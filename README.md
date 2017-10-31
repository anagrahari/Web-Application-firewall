# Web-Application-firewall
New Apache Module. C programming

A Web Application Firewall (WAF) is an HTTP application firewall, which sits in front of the application server and monitors all the incoming traffic. 
The Web Application Firewall provides a layer of cover to the web server against various attacks. The WAF monitors and filters out 
malicious traffic, and only allows legitimate traffic to go through. The WAF can guards the server form known XSS attacks, SQL injection 
attacks and can also filter out requests that vastly deviate from previously seen requests. The WAF is independent of the server it is 
protecting and can be implemented as a Reverse Proxy for a server, or as an Apache module. The WAF in this project has been implemented as 
an Apache module for the Apache HTTP server.

o	Signatures
The WAF scans the request and checks for the presence of attacks based on the rules defined in the signatures. The signatures store contains rules for the WAF to check for, in an encoded format, against known attacks like XSS, SQL injection etc. If any new attacks become known, we can add the signature of that attack to the signature store based on the proper encoded format, which will then be picked up by the WAF for subsequent requests. 
Examples for signatures:
HEADER:User-Agent,CONTAINS:"bot"	
REQUEST_METHOD:GET,PARAMETER:*,CONTAINS:"union all select"
REQUEST_METHOD:POST,PARAMETER:foo,CONTAINS:"../../../../"

o	Anomaly Detection
In anomaly detection, the WAF first learns how the legitimate traffic looks like. Based on this collected information, the WAF makes calculations about legitimate requests and proceeds to discard all the requests which far deviates from the norm.
Anomaly detection can be split into two phases, training phase and detection phase. In training phase, the WAF does not stop any requests allowing all the requests to hit the server. The purpose of this phase is only to collect information regarding the requests. The collected data is used to calculate the following parameters,
o	Maximum number of parameters for all requests across all pages.
o	Maximum number of parameters for each request.
o	Average length of every specific parameter for each request.
o	Character sets (allowed characters) of every specific parameter for each request.
In the detection phase, the WAF monitors the incoming requests based on the calculations. Requests which exceed the maximum number of 
parameters across all pages or for a page, are not allowed to go through. Requests that have parameter lengths which fall outside the 
range of average +-  3 * standard deviation are also dropped by the WAF. Requests with parameters having characters not seen in character 
set of the request parameter are ignored too.
