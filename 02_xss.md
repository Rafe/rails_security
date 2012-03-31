##What is Cross Site Script?

[http://en.wikipedia.org/wiki/Cross-site_scripting](http://en.wikipedia.org/wiki/Cross-site_scripting)

##CSS on Rails

The Cross Site Script attack is a huge threat of websites, back in Rails 2, you can call the "h" helper method which is alias of html_escape to protect your output:	<%= h @output %>
	Because the CSS attack is becoming a main threat on web application, Rails 3 set the HTML escape by default, so we don't need to remember escaping output. The view will be handle by the appropriate view handler and render to Html. During the rendering, string data will be parsed out special character and encoded to the escaped HTML character.
In ERB:Util Module, we can see how Rails parse the string:ActiveSupport\lib\active_support\core_ext\string\output_safety.rb	HTML_ESCAPE = { '&' => '&amp;',  '>' => '&gt;',   '<' => '&lt;', '"' => '&quot;' }
    JSON_ESCAPE = { '&' => '\u0026', '>' => '\u003E', '<' => '\u003C' }
    HTML_ESCAPE_ONCE_REGEXP = /[\"><]|&(?!([a-zA-Z]+|(#\d+));)/
    JSON_ESCAPE_REGEXP = /[&"><]/
	...
		def html_escape(s)
      s = s.to_s
      if s.html_safe?
        s
      else
        s.encode(s.encoding, :xml => :attr)[1...-1].html_safe
      end
    end
    
    ... 
    
    def html_escape_once(s)
      result = s.to_s.gsub(HTML_ESCAPE_ONCE_REGEXP) { |special| HTML_ESCAPE[special] }
      s.html_safe? ? result.html_safe : result
    end
From the source, we can know how do Rails escape the special character to HTML escaped character. 
Rails use the String encode method to escape html charactors.

	s.encode(s.encoding, :xml => :attr)[1...-1].html_safe
	Another interesting part is that every string in Rails have html_safe? method, which will return boolean that show the string is html safe or not. This is a extension defined in ActionSupport/core_ext/string/ , in ruby, you can easily extend the basic type when it's needed.
This expansion provide every string to be escaped before output.On the other side, if we want to allow HTML tag in output, we can call "raw" method in html helper to output raw string.
	<%= raw @html_content %>but these string might also infected by XSS string, so we can also call "sanitize" method before output Html string.
	<%= sanitize @html_content %>
sanitize is can strip out the dangerous tag, it can be set as white-list or black-list based, default tag is showed in the source code:
In actionpack\lib\action_controller\vendor\html-scanner\html\sanitizer.rb
	# Specifies the default Set of tags that the #sanitize helper will allow unscathed.	self.allowed_tags = Set.new(%w(strong em b i p code pre tt samp kbd var sub	sup dfn cite big small address hr br div span h1 h2 h3 h4 h5 h6 ul ol li dl dt dd abbr	acronym a img blockquote del ins))