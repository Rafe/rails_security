#What is CSRF?

http://en.wikipedia.org/wiki/Cross-site_request_forgery

#CSRF in Rails

CSRF is another threat that a website might have, in Rails, it provide a build-in CSRF protection function. We can activate it by calling "protect_from_forgery" in controller:
	class ApplicationController < ActionController::Base	  protect_from_forgery	end

Every form in the controller will be added a validation token , this token isstored as a random string in the session, which an attacker does not have access. When a request reaches the application, Rails then verifies the received token with the token in the session. The layout also need to including "csrf_meta_tags" in html head
In a sample application's form:
	<head>		<meta name="csrf-param" content="authenticity_token"/>		<meta name="csrf-token" content="b3wvDdaaA9t0W0jgdP0nW0T12jhCohYJ67oBTQ/P6K0="/>
	</head>	<form accept-charset="UTF-8" action="/en/orders/298486374" class="edit_order" id="edit_order_298486374" method="post">		<div style="margin:0;padding:0;display:inline">			<input name="utf8" type="hidden" value="&#x2713;" />			<input name="_method" type="hidden" value="put" />			<input name="authenticity_token" type="hidden"				value="b3wvDdaaA9t0W0jgdP0nW0T12jhCohYJ67oBTQ/P6K0="/>		</div>	</form>the token is generated and saved in session,  

In actionpack\lib\action_controller\metal:

	def verified_request?	  !protect_against_forgery? || request.forgery_whitelisted? || 
	    form_authenticity_token == params[request_forgery_protection_token]	end	# Sets the token value for the current session.	def form_authenticity_token	  session[:_csrf_token] ||= ActiveSupport::SecureRandom.base64(32)	end