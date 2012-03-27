# Rails Security Topics

Ruby on Rails is an innovative web application framework based on Ruby. It was created by DHH(David Heinemeier Hansson) since 2004. Until now, it already have 3 major version and several restructure. It is used by lots of major website and handling billions of requests on different services, like Twitter, Github, Groupon. 

However, as it goes popular and use on website that handle huge traffics, the security become more and more important. On both general web applcation vulnerability and the framework specific exploits. How Rails handle those topic is this project will like to explore.


##Source structure

Ruby on Rails is composite by 5 seperate package, include :

* ActionPack : The core of RoR, It provides Controller and Views Layer, handle and respond requests and route the request to actions.
* ActiveModel : The Model layer of RoR, define object that can interact with action pack.
* ActiveSupport : The common utility classes and library for RoR.
* ActiveRecord : The Object Relation Mapping of RoR, adapt different database to Rails Model.
* ActionMailer : The email service of RoR.

##Security Policy

RoR provides their security policy on [http://rubyonrails.org/security](http://rubyonrails.org/security).  For reporting a bug, you need to send e-mail to security@rubyonrails.org.  The email will deliver to a subset of the core team who handle security issues, and will be acknowledge within 24 hours, and respond how they will handle in 48 hours. Rails also have disclosure policy to protect the information until the fixed is ready, and send the security announcement and patches by [mailing list](http://groups.google.com/group/rubyonrails-security). Once the security patch is complete, user can update the framework by rubygem.
##Topics:
1. [Finger Print](https://github.com/Rafe/rails_security/blob/master/01_fingerprint.md)
2. [Cross Site Script](https://github.com/Rafe/rails_security/blob/master/02_xss.md)
3. [Cross-site Request Forgery](https://github.com/Rafe/rails_security/blob/master/03_csrf.md)
4. [Session](https://github.com/Rafe/rails_security/blob/master/04_session.md)
5. [Sql Injection](https://github.com/Rafe/rails_security/blob/master/05_sql_injection.md)
6. [Mass Assignment](https://github.com/Rafe/rails_security/blob/master/06_mass_assignment.md)
7. [Javascript Hijection](https://github.com/Rafe/rails_security/blob/master/07_javascript_hijacking.md)

		
		