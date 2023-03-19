# No Rate Limit
1. Intercept while creating a account and send the request to intruder and add payload and see if you are able create multiple accounts
2. No Rate limit to OTP using intruder we can do account take over, OTP can only be used once
3. adding nullbyte (%00) pr (%0d%0q) or (%09)at the end of email after rate limit
4. Try Host header injection (changing host or added X-Forwared-Host )
5. Try X-Forwarded-For to Spoof Originating IP 
6. Trying with X-Forwarded-For: IP Header 2x times Instead of One time, Bypass Ratelimit Protection
7. check all this X-Originating-IP, X-Forwarded-For, X-Remote-IP, X-Remote-Addr, X-Client-IP, X-Host, X-Forwared-Host
8. check at password reset, otp,signup,password function,token 