##Mass assignment

The mass-assignment is a specific problem in Ruby on Rails, it allows an attacker to set any model's attribute by manipulating the hash passed to a model's value:In the Html form, the parameter is like:
	<form...>	  <label for="user_name">Name</label>:	  <input id="user_name" name="user[name]" size="40" type="text" value="dave" />	  <label for="user_password">Password</label>:	  <input id="user_password" name="user[password]" size="40" type="password" />	</form>
And the ActiveRecord will automatically mapping the attribute to database model
	def signup	  @user = User.new(params[:user])	end
Therefore by setting other attributes, attacker can manipulate the value in database:
	HTTP 1.1 POST/
	User[name]="user"&user[password]="password"&user[admin]='true';

The counterpart of this problem, is to set the restricted list :

	class User < ActiveRecord::Base
	  attr_protected :admin
	  
	  #or we can also set the white list:
	  attr_accessible :name, :password
	  
	  ...
	  
	end		

	


