## Lesson 01 - Intro and Lab Setup

We will be using the **c**ompletely **r**idiculous **API** (crAPI) application stack for this training. crAPI is an intentionally vulnerable web application stack for learning about API security managed by OWASP.

- crAPI GitHub page: [OWASP/crAPI: completely ridiculous API (crAPI) (github.com)](https://github.com/OWASP/crAPI)
- crAPI Challenges: [crAPI/docs/challenges.md at develop · OWASP/crAPI (github.com)](https://github.com/OWASP/crAPI/blob/develop/docs/challenges.md)


In order to explore the crAPI vulnerabilities we will use Burpsuite Community Edition (Burp). You could also use Postman, Hopscotch or any other web proxy application that allows you to intercept and modify requests. Burp Suite is a tool commonly used by penetration testers and assessors because of its ability to decode hashes, modify JWT tokens, send requests repeatedly with small changes, fuzzing. It is extendable with plugins and the Pro version includes additional features including an automatic scanner to look for common vulnerabilities.

To speed up the process, we have created a Cloud Lab with crAPI and the tools you need pre-installed. The Cloud Lab is based on a Debian distribution designed to be small but easy to use. 

## Startup the Cloud Lab:

1. Each team member will need to reset their password and then login to MetaCTF (https://app.metactf.com/cloud). Once logged in, they need to select “Cloud Labs” to gain access to the system.

   ![A screenshot of a computer  Description automatically generated](Files/clip_image001.png)

2. At that point, they can start the “OWASP Practice VM” and wait for the RDP button.

   ![A screenshot of a computer  Description automatically generated](Files/clip_image001-1715220574544-3.png)

3. This will open up a new window with access to the VM.

4. After connecting by RDP, you will need to wait until the full VM loads including the taskbar at the bottom of the screen.

Everything should be ready to start training at this point.

### Few things to get you started:

**Username:** _scudev_

**Password:** _scudev_

The user is part of the sudo group and can perform administrative tasks.

We will be “proxying” all of our traffic through Burp. When you start Burp, just create a temporary project and start it.

For simplicity, we will use the Burp built in browser. This will proxy all of our traffic through Burp allowing you to see the requests and responses.

![](Files/image%202.png)  

  

crAPI will have the following applications preinstalled:

- A microservice architecture, API driven, web application on port 8888/tcp. You can browse to it using localhost:8888
- Mailhog: A fake webmail application that receives all email from the application. Accessible on port 8025. You can browse to it using localhost:8025


### First Steps:

1. Startup Burp (ignore any application updates for now)
- Select temporary project in memory and then “next”.
- Select “Start Burp”
3. Start the Proxy browser
- Click on the menu tab “Proxy” and then on “Open browser”
- Browse to localhost:8888
- If you do not see the crAPI login screen, then we need to start crAPI and then refresh the page. 

  - Open a terminal

  - ``` sudo docker-compose -f docker-compose.yml --compatibilitymode
    sudo docker-compose -f /home/scudev/docker-compose.yml --compatibility up -d
5. Create an account so you can explore
- Select “SignUp”
- Fill in the form with any fake information you like. (note: all fields must be filled in).

7. Login with the newly created account
8. Add a vehicle

- Select the “Click here” link in the banner.

  ![](Files/image%204.png)
- Open a new tab and browse to Mailhog at localhost:8025
- Find the “Welcome to crAPI” email and open it to find your VIN and PIN. Take note as you will need these to add a vehicle.
- Now click on the “+ Add a Vehicle” link and fill out the form with the VIN and PIN provided in the email.
10. At this point, you should have an account with a vehicle like any other new user just getting started with the application.

![](Files/image%205.png)

12. Look at your Proxy History

- Switch back to Burp Suite Community (ctrl-tab will switch between apps quickly).
- Click on the “HTTP history” tab.
- Click on the downward facing arrow next to the hashtag column to reverse sort by request ID.

  ![](Files/image%206.png)
- Note that you will see requests made by your browser when it loads or interacts with the application.
14. Filter traffic to only what is in scope
- Click on the “Target” menu tab.
- Right click on the “http://loclhost:8888/” item
- Select “Add to scope”

  ![](Files/image%207.png)
- Click back to the “Proxy” tab.
- Click on the Filter bar.
- Check the “Show only in-scope items”

  ![](Files/image%208.png)
- Select “Apply”
16. You should only be seeing requests related to the crAPI application now.

    ![](Files/image%209.png)
- Note: You may see colored lines indicating a JWT token was included with the request.

You are all set to start the challenges. Enjoy!!!

### Misc

The following is a list of API endpoints pulled from the Javascript files included with the application:

            LOGIN: "api/auth/login",
            GET_USER: "api/v2/user/dashboard",
            SIGNUP: "api/auth/signup",
            RESET_PASSWORD: "api/v2/user/reset-password",
            FORGOT_PASSWORD: "api/auth/forget-password",
            VERIFY_OTP: "api/auth/v3/check-otp",
            LOGIN_TOKEN: "api/auth/v4.0/user/login-with-token",
            ADD_VEHICLE: "api/v2/vehicle/add_vehicle",
            GET_VEHICLES: "api/v2/vehicle/vehicles",
            RESEND_MAIL: "api/v2/vehicle/resend_email",
            CHANGE_EMAIL: "api/v2/user/change-email",
            VERIFY_TOKEN: "api/v2/user/verify-email-token",
            UPLOAD_PROFILE_PIC: "api/v2/user/pictures",
            UPLOAD_VIDEO: "api/v2/user/videos",
            CHANGE_VIDEO_NAME: "api/v2/user/videos/<videoId>",
            REFRESH_LOCATION: "api/v2/vehicle/<carId>/location",
            CONVERT_VIDEO: "api/v2/user/videos/convert_video",
            CONTACT_MECHANIC: "api/merchant/contact_mechanic",
            RECEIVE_REPORT: "api/mechanic/receive_report",
            GET_MECHANICS: "api/mechanic",
            GET_PRODUCTS: "api/shop/products",
            GET_SERVICES: "api/mechanic/service_requests",
            BUY_PRODUCT: "api/shop/orders",
            GET_ORDERS: "api/shop/orders/all",
            GET_ORDER_BY_ID: "api/shop/orders/<orderId>",
            RETURN_ORDER: "api/shop/orders/return_order",
            APPLY_COUPON: "api/shop/apply_coupon",
            ADD_NEW_POST: "api/v2/community/posts",
            GET_POSTS: "api/v2/community/posts/recent",
            GET_POST_BY_ID: "api/v2/community/posts/<postId>",
            ADD_COMMENT: "api/v2/community/posts/<postId>/comment",
            VALIDATE_COUPON: "api/v2/coupon/validate-coupon"
