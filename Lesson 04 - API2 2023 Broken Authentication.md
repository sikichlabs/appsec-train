## Lesson 04 - API2: 2023 Broken Authentication

"Authentication mechanisms are often implemented incorrectly, allowing attackers to compromise authentication tokens or to exploit implementation flaws to assume other user's identities temporarily or permanently. Compromising a system's ability to identify the client/user, compromises API security overall."
[OWASP Top 10 API Security Risks – 2023 - OWASP API Security Top 10](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)

Reference: [API2:2023 - Broken Authentication](https://owasp.org/API-Security/editions/2023/en/0xa2-broken-authentication/)

## Take over another user's account

Broken authentication allows an attacker to gain unauthorized information about other user's accounts. And in the worse case scenario, take over the account fully. Some examples of Broken Authentication include:

* User enumeration through the forgot password functions.
* Session fixation allowing the attacker to set the session id before a user logs in and then hijack the session.
* Broken MFA
* Change the password for another user's account.

We are going to use a combination of the last two (2) options for this example.

When we were browsing in the earlier labs, we noted other user information in the "Community" responses. Including Author ID and email addresses. So we will log out and take a look at the login page. You will notice that there is a "Forgot Password" button. If we use this to reset our own password, we will receive an email from the system with a four (4) digit one time password (OTP).

The problem with four (4) digit OTPs is that if the system does not have rate limiting or throttling we can quickly send  0001 to 9999 requests to discover the OTP for this user. As it turns out, crAPI does have rate limiting on version 3 of their API. But if there is a version 3, then there is probably a version 2. So we send our requests to change the password for another user using version 2 and find that there is no rate limiting in place. This allows us to get the code.

NOTE: We are using the Community Edition of Burp Suite which has throttling built in so that you will want to purchase the Pro version. So for this lab, we will work through the process but when we attempt to send multiple OTPs, we will view the email to find the real OTP and limit our search to a range that allows you to complete this lab more quickly.

#### Lab Steps

1. Open the Burp Suite browser, and click on the down arrow next to the user profile icon and then click on the "logout" menu item.

   ![image-20240506194248655](C:\Users\Thomas.Freeman\AppData\Roaming\Typora\typora-user-images\image-20240506194248655.png)

2. Enter your email address and tell it to send your OTP.

   ![image-20240506180653969](C:\Users\Thomas.Freeman\AppData\Roaming\Typora\typora-user-images\image-20240506180653969.png)

3. Now open another tab in your browser and go to http://localhost:8025

4. You will see a webmail portal. Click on the "crAPI OTP" message.

   ![image-20240506180121555](C:\Users\Thomas.Freeman\AppData\Roaming\Typora\typora-user-images\image-20240506180121555.png)

5. Note that the OTP is only 4 digits.

6. Go back to the crAPI application and reset your password.

7. Notice that in the Proxy HTTP History window, there is now a POST request to /identity/api/auth/v3/check-otp. And in the request is a JSON section with the email, OTP and password to be used. Since we are going to use this request to guess another user's OTP, right click on this row and click on "Send to Intruder". The request will sit in Intruder while we do some more work to prepare for this attack.

   ![image-20240506181202641](C:\Users\Thomas.Freeman\AppData\Roaming\Typora\typora-user-images\image-20240506181202641.png)

8. Now lets do the same thing but this time for one of the users we noticed in the "Community" forum response. I am going to use robot001@example.com

9. At this point, the real user should receive an email with a 4 digit OTP. We would not know it and would need to guess every number from 0001 to 9999 until we found the right OTP. For this example, to save time, we are going to cheat and look at that OTP since all emails will be in our webmail system.

10. In my case, the OTP is 2709

    ![image-20240506182718044](C:\Users\Thomas.Freeman\AppData\Roaming\Typora\typora-user-images\image-20240506182718044.png)

11. Now we are all set, go to the "Intruder" window. Make sure that the attack type is "Sniper".

12. Change the v3 to v2.

13. Change the email to robot001@example.com.

14. Now we need to tell Burp Suite where the OTP is that it should change out with incrementing numbers. We do this by highlighing the OTP number and then selecting "Add §". This will put the "§" mark on each side of our OTP number.

    ![image-20240506182845689](C:\Users\Thomas.Freeman\AppData\Roaming\Typora\typora-user-images\image-20240506182845689.png)

15. Now click on the "Payloads" menu item.

16. We need to change the "Payload type" to Numbers. Make the "From" and "To" be a 5 digit range that includes the OTP you know from the email. (Again, in a real attack, we would not know that number but using the Pro version of Burp Suite would make the attack very quick to search all 9999 numbers.) Finally, change the "Min integer digits" to 4 and select "Start Attack'"

    ![image-20240506182951385](C:\Users\Thomas.Freeman\AppData\Roaming\Typora\typora-user-images\image-20240506182951385.png)

17. You will see a popup window that will show each request being made. If you note the "Status Code" column, you will see 500 codes for all the failed requests and then you will see a 200 code for the successful request. Click on that row and then click on the response.

    ![image-20240506183149389](C:\Users\Thomas.Freeman\AppData\Roaming\Typora\typora-user-images\image-20240506183149389.png)

18. You may notice also that the "Response Received" is much longer for the successful entry. This is the basis for "timing attacks". An attacker may send a lot of requests using different email addresses that they do not know if they are valid or not. If there is a large difference in timing for some of the requests, that typically means that they have discovered valid email addresses that can be used in an attempt to guess passwords or perform password resets.

19. We have successfully reset the password for robot001@example.com

20. Challenge complete.

> **Please Note:** Don't login to the account you compromised. There appears to be a problem with some of the accounts. If you attempt to login to robot001@example.com, you will get a white screen of death and need to logout to get back to your account. To correct this, you will need to clear the browser cache, history and cookies by selecting Ctrl-Shift-Delete.

