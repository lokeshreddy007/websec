Browsers utilize headers sent over HTTP (secure HTTP connections mostly) to enforce and confirm such communication standards as well as security policies. Making use of these HTTP headers to increase security for the code running on the browser client-side is a quick and efficient method to mitigate security vulnerabilities and add defense in depth

## The HTTP security headers

### Strict-Transport-Security

HTTP Strict Transport Security, also known as HSTS, for short. Enforces a secure communication channel to the web server.
  
Strict Transport Security (HSTS), as a browser security control to allow only HTTPS-enabled resources to be fetched from the primary domain for a website.(upgrade the request to HTTPS)

Reviewing Chrome’s HSTS settings  navigate to `chrome://net-internals/#hsts` in Chrome’s address bar and update as necessary

### X-Frame-Options

 X Frame Options header defines the policies of rendering a web page as an HTML frame.

The X-Frame-Options* HTTP header was introduced to mitigate an attack called Clickjacking. It allows an attacker to disguise page elements such as buttons, and text inputs by hiding their view behind real web pages which render on the screen using an iframe HTML element or similar objects.

`Deprecation notice` The X-Frame-Options header was never standardized as part of an official specification but many of the popular browsers today still support it. Its successor is the `Content-Security-Policy (CSP)` header which will be covered in the next and one should focus on implementing CSP for newly built web applications

To mitigate the problem, a web server can respond to a browser’s request with an X-Frame-Options HTTP header which is set to one of the following possible values:

1. `DENY` - Specifies that the website can not be rendered in an iframe, frame, or object HTML elements.
2. `SAMEORIGIN` - Specifies that the website can only be rendered if it is embedded on an iframe, frame, or object HTML elements from the same domain the request originated from.
3. `ALLOW-FROM <URI>` - Specifies that the website can be framed and rendered from the provided URI. It is important to note that you can’t specify multiple URI values, but are limited to just one.

A few examples to show how this header is set are:

```bash
X-Frame-Options: ALLOW-FROM http://www.mydomain.com
# or
X-Frame-Options: DENY
```

### Content-Security-Policy

Content Security Policy, or CSP for short, defines a wide range of security policies for web browsers.

Another improvement to the previous set of headers we reviewed so far is a header that can tell the browser which content to trust. This allows the browser to prevent attempts of content injection that are not trusted in the policy defined by the application owner.

With a Content Security Policy* (CSP) it is possible to prevent a wide range of attacks, including Cross-site scripting and other content injections. The implementation of a CSP renders the use of the X-Frame-Options header obsolete

Examples of some of the issues it can prevent by setting a CSP policy:

- `Inline JavaScript code` specified with `<script>` tags, and any DOM events which trigger JavaScript execution such as
onClick() etc.
- `Inline CSS code` specified via a `<style>` tag or attribute elements

With CSP allowlists, we can allow many configurations for trusted content, and as such the initial setup can grow to a set of complex directives

Let’s review one directive called connect-src. It is used to control which remote servers the browser is allowed to connect to via XML-
HttpRequest (XHR), or `<a>` elements. Other script interfaces that are covered by this directive are: Fetch, WebSocket, EventSource,
and Navigator.sendBeacon().

Acceptable values that we can set for this directive:

- `none` - not allowing remote calls such as XHR at all.
- `self` - only allow remote calls to our domain (an exact domain/hostname - sub-domains aren’t allowed).
  
An example for such content security policy being set is the following directive which allows the browser to make XHR requests to
the website’s own domain and Google’s API domain:

```bash
Content-Security-Policy: connect-src 'self' https://apis.google.com;
```
Another directive to control the allowlist for JavaScript sources is called `script-src`. This directive helps mitigate Cross-Site-Scripting

(XSS) attacks by informing the browser which sources of content to trust when evaluating and executing JavaScript source code. `script-src` supports the ‘none’ and ‘self’ keywords as values and includes the following options:

- `unsafe-inline` - allow any inline JavaScript source code such as `<script>`, and DOM events triggering like onClick() or
javascript: URIs. It also affects CSS for inline tags.
- `unsafe-eval` - allow execution of code using eval()
  
For example, a policy for allowing JavaScript to be executed only from our own domain and from Google’s, and allows inline
JavaScript code as well:

```bash
Content-Security-Policy: script-src 'self' https://apis.google.com 'unsafe-inline'
```
Note, the 'unsafe-inline' directive refers to a website’s own JavaScript sources.

A full list of supported directives can be found on the CSP policy
directives page on [MDN](https://developer.mozilla.org/en-US/docs/Web/Security/CSP/CSP_policy_directives) but let’s cover some other common options and their values.

- `default-src` - where a directive doesn’t have a value, it defaults to an open, non-restricting configuration. It is safer to set a default for all of the un-configured options and this is the purpose of the default-src directive.
- `script-src` - a directive to set which script sources we allow to load or execute JavaScript from. If it’s set to a value of ‘self’
then it will only allow sources from our own domain. Also, it will not allow inline JavaScript tags, such as `<script>`. To enable those, add 'unsafe-inline' too

#### Log CSP
```bash
Content-Security-Policy: default-src 'self'; report-uri https://mydomain.com/report
```
Note that the semicolon is added to end the content security policy directives, and begin a new report-uri directive.

Once added, the browser will send a POST request to the URI provided with a JSON format in the body for anything that violates the content security policy of only serving content from our own origin.

### Referer and Referrer Policy

When users browse through web pages, the browser may set a request header called Referer in certain conditions. This Referer header is often used by backend servers to track users’ behavior for analytics and other means.

What if a web page had stored sensitive information in a URL such as an account ID as part of the URL, or other sorts of sensitive information about the system? If a link on that page is then visited, and the browser sets the Referer header as it would normally, that could lead to sensitive information leakage.

This is where the Referrer Policy header comes in. This header, when set by a web server, instructs the browser whether to populate the Referer header when navigating out of that web page and into a new one

**`The Referrer Policy`** header can set one of the following policies that instruct the browser’s behavior when navigating off the page:

- `no-referrer`: Instruct the browser to never set a Referer header at all, for any links related to requests from this web page.
- `no-referrer-when-downgrade`:If there’s a security downgrade in the form of making requests from an HTTPS website to an HTTP, then the browser doesn’t set the Referer header. this is indeed thedefault behavior that browsers follow if a referrer policy is unset, or invalid.
- `origin`:Instead of sending the full URL - the origin, path, and query parameters of the current page being navigated from, the browser will only send the origin, such as https://www.google.com and nothing beyond that in the URL
- `origin-when-cross-origin`:only the origin is sent to any requests the browser makes to navigate off the page, when those addresses match a cross-origin. Otherwise, when requests are made to URLs of the same origin (as complies with the same-origin policy), the default behavior of setting the Referer header to the current URL is followed
- `same-origin`:The current URL is set for the Referer header to any requests that are considered same-origin, otherwise, it isn’t set at all
- `strict-origin`: When requests are kept in the same origin, only the origin is set as the Referer value
- `unsafe-url`:which always sets a value for the Referer header and could lead to a sensitive information leak
- `strict-origin-when-cross-origin`
1.if there’s a security downgrade in the form of making requests from an HTTPS website to an HTTP, then the browser doesn’t set the Referer header at all
1. If the request is made to an HTTPS cross-origin address, then only the origin is set for the Referer header.
2. If the request is made to the same origin, then the full URL is set
  

### X-XSS-Protection 

This security headers were originally introduced as part of specific web browser software, to combat security threats such as Cross-site Scripting, and MIME Sniffing, and have since been `deprecated in favor of better security controls`

The Cross-site Scripting protection header instructs the browser to set specific XSS-mitigating policies.

The HTTP header X-XSS-Protection is used by IE8 and IE9 and allows toggling on or off the Cross-Site-Scripting (XSS) filter capa- bility that is built into the browser. 

Turning XSS filtering on for any IE8 and IE9 browsers rendering your web application requires the following HTTP header to be sent
```bash
X-XSS-Protection: 1; mode=block
```
### X-Content-Type-Options

The X Content Type Options is a browser-specific header to instruct the browser to apply strict settings to the Content-Type value of the response.

When browsers fetch remote sources of content, such as JavaScript or images, they are instructed using the Content-Type header on the type of content

For example, when a PDF content type is fetched by the browser, the server hints the browser about it by setting the following header: 
```bash
Content-Type: application/pdf
```
These content types are standardized by the IANA organization as MIME types, and a full list of common MIME types can be seen [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Common_types). 

What happens when the browser is instructed an incorrect MIME type, or not at all entirely? 

In such a case, the browser will attempt to guess the content type by reading and interpreting the content data. This action is referred to as MIME Sniffing.

The purpose of this header is to instruct the browser to avoid guessing the web server’s content type which may lead to an incorrect render than that which the webserver intended. 

The `X-Content-Type-Options` HTTP header is used by IE, Chrome, and Opera and is used to mitigate a MIME-based attack. [Common_types](†https://mimesniff.spec.whatwg.org/)

```bash
X-Content-Type-Options: nosniff 
```

1. [Httparchive](https://httparchive.org/reports/state-of-the-web#pctVuln)
2. [wWebpagetest](https://www.webpagetest.org/)
3. [Check my header](https://github.com/UlisesGascon/check-my-headers)
4. Chrome light house

The web is an evolving standard and as such, new security controls would be introduced. We should keep an eye on them! Embrace and prepare for privacy, feature controls, and future headers such as Referrer-Policy, Feature-Policy, Origin-Policy, Integrity, Accept-CH, and Clear-Site-Data.