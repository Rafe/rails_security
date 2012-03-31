##Javascript Hijacking

  JavaScript hijacking is a problem that when the server use ajax to send request to server, if the page is XDD vulnerability, attacker can set script to overwrite the Native Object like "String" , steal or change the data between client and server.
  
	//With the script of:	<script src="http://my.bank.evil/transactions.json"></script>
	//And JSON response:	[[account:"richman",amount:1000000,to:"someone"]]

The JSON respond might send back to the evil site when loading the script.The solution of this problem is:
First, make sure the page is XSS protected, because XSS is the root of all evil.Second, make sure the connection code is writting in closure, and pass the critical object to prevent overwrite
	(Function(window){	  //...some action	  var str = new String();	})(window)
Or we can also add some garbage data between return value to disable the hijack function:
	===Some garbage padding ====	[[account:"richman",amount:1000000,to:"someone"]]