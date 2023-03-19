# OWASP Top 10 - 2021
1. Broken Access Control
2. Cryptographic Failures
3. Injection
4. Insecure Design
5. Security Misconfiguration
6. Vulnerable and Outdated Components
7. Identification and Authentication Failures
8. Software and Data Integrity Failures
9. Security Logging & Monitoring Failures
10. Server-Side Request Forgery (SSRF)

## Broken Access Control
Websites have pages that are protected from regular visitors. For example, only the site's admin user should be able to access a page to manage other users. If a website visitor can access protected pages they are not meant to see, then the access controls are broken.

```Access control, sometimes called authorization, is how a web application grants access to content and functions to some users and not others. These checks are performed after authentication, and govern what ‘authorized’ users are allowed to do.```

A regular visitor being able to access protected pages can lead to the following:

- Being able to view sensitive information from other users
- Accessing unauthorized functionality

```Interest Read``` [Youtube private video stealing](https://bugs.xdavidhu.me/google/2021/01/11/stealing-your-private-videos-one-frame-at-a-time/)

###  Insecure Direct Object Reference (IDOR)
Insecure Direct Object References occur when an application provides direct access to objects based on user-supplied input. As a result of this vulnerability attackers can bypass authorization and access resources in the system directly, for example database records or files.

### Differnce betwwen  IDOR and BAC

`Insecure Direct Object Reference` is generally where a database object has an ID exposed to the client. So for example in an app using REST style URLs we could have

```js
http://myapp.test/users/2
```
showing your user account and then if you change to

```js
http://myapp.test/users/4
```
it shows you someone elses account. In this instance you would need to be logged in, but AFAIK there's nothing in the vulnerability specification that says that you accessing for example

```js
http://myapp.test/page/12
```
whilst unauthenticated wouldn't be insecure DOR assuming that the page is only meant to be accessed while logged in.

`Broken access control` is a more general case where a control over access to application functionality isn't correctly controlled, but without the requirement for it to relate to accessing a database object.

So for example.

```js
http://myapp.test/super_secret_admin_panel
```
being accessible without authentication would be a broken access control. but you could have a situation where it relates to authenticated users getting access to functionality they shouldn't, so an ordinary staff member accessing
```js
http://myapp.test/all_staff_salary_details
```
when this should only be accessible by members of the HR department.


## Cryptographic Failures

A cryptographic failure refers to any vulnerability arising from the misuse (or lack of use) of cryptographic algorithms for protecting sensitive information. Web applications require cryptography to provide confidentiality for their users at many levels.

## Injection
Injection flaws are very common in applications today. These flaws occur because the application interprets user-controlled input as commands or parameters. Injection attacks depend on what technologies are used and how these technologies interpret the input. Some common examples include:

`SQL Injection`: This occurs when user-controlled input is passed to SQL queries. As a result, an attacker can pass in SQL queries to manipulate the outcome of such queries. This could potentially allow the attacker to access, modify and delete information in a database when this input is passed into database queries. This would mean an attacker could steal sensitive information such as personal details and credentials.
`Command Injection`: This occurs when user input is passed to system commands. As a result, an attacker can execute arbitrary system commands on application servers, potentially allowing them to access users' systems.
