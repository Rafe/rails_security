#Session

Http is a stateless protocol, so server is using session id in cookie to distinguishdifferent connections. When a user connection to server, it will assign a session id to the user, than when the user connect again, server will load the exist session for the user. And therefore, attacker might sniff the session id to duplicate user authorization.
##Rails session
In Rails, session id is a random Hex number string:
In /actionpack/lib/action_dispatch/middleware/session/abstract_store.rb
	def generate_sid	  ActiveSupport::SecureRandom.hex(16)	end
So it is not feasible to brute-force Rails' session ids.
##Session Values
By default, ruby save session values in cookie, because get value from request header is much faster and require less memory than getting it from hash. But It also introduce security problem that some important session value, such as user_id, also saved in the client with plain code.To prevent client modified session values in cookie, Rails encrypt the cookie with SHA1, HMAC and encode to 64base with a secret key:
save the cookie encryption key in /config/initializer.rb. encoded it with HMAC method and AES and encode to 64 base.
In activesupport/lib/active_support/message_encryptor:
	def initialize(secret, cipher = 'aes-256-cbc')	  @secret = secret	  @cipher = cipher	end
	def encrypt(value)	  cipher = new_cipher	  # Rely on OpenSSL for the initialization vector	  iv = cipher.random_iv	  cipher.encrypt	  cipher.key = @secret	  cipher.iv = iv	  encrypted_data = cipher.update(Marshal.dump(value))	  encrypted_data << cipher.final	  [encrypted_data, iv].map {|v| ActiveSupport::Base64.encode64s(v)}.join("--")	end
and the secure key is saved in Rails application, in config/initializer/secret_token.rb:
	Rails.application.config.secret_token = '21d0fd931f5c004bf7c6c1dbadeb0271123557a574f21d744399d91c6a47c1ee14eeabd5eddd2d5ef8a07d4cf2b705fd00b48d6f4a3ae22c9b86b0822b563e7a'
and the length is restricted to 30 Char length. So it is hard to brute force decrypt. But it could also be dangerous if attacker get the encryption key under your application folder. So for the security, do not save important data in session values.
##Session fixation
Session fixation is a attack method by fixing user's session id to a known id, and thereby the attacker can assess the server as user using the same session Id.
First, the attacker get a connection and maintain a session id. And than assign the session id to other user by JavaScript or query string.
For example, a Session fixation XSS might like this:
	<script>document.cookie="_session_id=16d5b78abb28e3d6206b60f22a03c8d9"; </script>
after user login, attacker can also login by using the same session_id because the authorize information is saved in the session.
##Rails with session fixation:The solution to keep session save, is remember to call reset session after userlogin or set the expiration time on session.
For example,in a UserController:
	def login	  reset_session	  sign_in	end

By default, Rails saves session data in cookie, thereby the user authorize data is saved in the client side, therefore even if the attacker fixed the session id, they still can't access the data. But in the counterpart, the attacker can also get the authorize data and try to decrypt and fake it.
The storage setting is in Rails application: config/initializer/session_store.rb
	SampleApp::Application.config.session_store :cookie_store, :key => 'sampleApp_session'	

