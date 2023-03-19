# OTP Bypass (Captcha)

1. Update response from server to ok,1,0, success, ture -> Bug if the check is done at client side instead at server side
2. Check code to see any static otp is used or try 0000 or 1111, 1234 or xxxx, xyz
3. Remove the response and send 1 (if no response from the server send 1)