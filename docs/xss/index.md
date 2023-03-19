# XSS
1. Use xss payload in all the input fields, and check the source code to get clear how website is rendering the payload
2. Xss in request header (also change request type and add `referer:http://xyz.com/?q=hello"><script>alert(1)</script>), `referer payload has be formed based on the response from the server 
3. Xss using useragent, if the server is caching then save useragent  for the url with xss payload and sent that url 
4. In email feild tru to use this payload `"><svg/onload=confirm(1)>"@x.y`
5. Xss using base64 encoding
6. Use burp spider or paramcheck to find the search params location to using in xss
7. Store xss try in edit profile in username and other...
