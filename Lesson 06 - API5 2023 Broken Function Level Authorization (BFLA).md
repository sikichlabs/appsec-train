## Lesson 06 - API5: 2023 Broken Function Level Authorization (BFLA)

"Complex access control policies with different hierarchies, groups, and roles, and an unclear separation between administrative and regular functions, tend to lead to authorization flaws. By exploiting these issues, attackers can gain access to other users’ resources and/or administrative functions."
[OWASP Top 10 API Security Risks – 2023 - OWASP API Security Top 10](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)

Reference: [API5:2023 - Broken Function Level Authorization](https://owasp.org/API-Security/editions/2023/en/0xa5-broken-function-level-authorization/)

We have seen that we can access an object (BOLA) without authorization, we can access an object's properties (BPLA) without authorization. Now let's see if we can implement an objects' methods without authorization. If the video object contains all the data related to the video and all the methods (functions) needed to manipulate the object, then we may be able to access the method directly.

## Delete another user's video

In the last exercise, you saw that we could change the HTTP request. And you noted that when we can view videos based on an ID in the URL. Let's combine these to delete another user's video.

#### Lab Steps

1. Using the Burp Suite Browser logout of your current user and create a new user.

2. Login as the new user and add a profile video. Then rename the video.

3. Now logout of the new user and log back into your previous user.

4. Open the Proxy HTTP History window and search for the PUT request for URL /identity/api/v2/user/videos/ and send it to "Repeater."

5. In Repeater, change the PUT request to a DELETE request and click "Send".

   ![image-20240506233401678](C:\Users\Thomas.Freeman\AppData\Roaming\Typora\typora-user-images\image-20240506233401678.png)

6. In this case, you have to be an Admin to perform this function...or do you.

7. Notice that the request URL has the endpoint "user" in it. What if we change that to admin so that we are using the admin endpoint?

   ![image-20240506233546295](C:\Users\Thomas.Freeman\AppData\Roaming\Typora\typora-user-images\image-20240506233546295.png)

8. Bingo. We were able to use the video's "DELETE" method to delete the video without proper authorization.

9. Challenge completed.

