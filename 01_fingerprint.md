#How to fingerprint application using Rails?

Fingerprint, means to find what technology stacks that the server use. For hacker, it is important to know what language and framework is used on the server. Moreover, the version of the framework and what plugin or gem used, will be a important information for hacker that they can find vulnerability on these frameworks and gems.

Here's the method to find out the Rails fingerprint:
##Folder Name:The Rails application have a lot of preference about what directory should be named, the structure and dir is predefined by Rails, which is one of the Rails philosophy "Convention over Configuration". However, some the folder name is rarely used by the other framework, so we can know the website is using Rails by the naming of directory. For example, the folder name javascripts/, stylesheets/, is the Rails specific names for javascripts file and stylesheets file.
##Error page
the error page is named as the response code like 404.html. We can also distinguish Rails Application by these naming. ##Server Header:By seeing the response header, we can see some rail module is used, like mod_rails/mod_rack. As we know that the server is running Rails.

for example, from the response header of getharvest.com:

	HTTP/1.1 304 Not Modified
	Server: nginx
	Date: Fri, 30 Mar 2012 22:43:00 GMT
	Connection: keep-alive
	Status: 304
	X-Powered-By: Phusion Passenger (mod_rails/mod_rack)
	P3P: policyref="http://www.getharvest.com/p3p.xml", CP="IDC DSP COR ADM DEVi TAIi PSA PSD IVAi 	IVDi CONi HIS OUR IND CNT"
	ETag: "606944f07102a70e3978fa3ffb61743d"
	Cache-Control: max-age=0, private, must-revalidate
	X-UA-Compatible: IE=Edge,chrome=1
	X-Runtime: 0.029377
	X-Harvest-Box: rails2
	X-Harvest-FE: lb1
	X-Harvest-DC: vxny

We can know that it is using Phusion Passenger and running rails.