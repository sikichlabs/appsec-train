## Lesson 05 - API3: 2023 Broken Object Property Level Authorization (BOPLA)

"This category combines [API3:2019 Excessive Data Exposure](https://owasp.org/API-Security/editions/2019/en/0xa3-excessive-data-exposure/) and [API6:2019 - Mass Assignment](https://owasp.org/API-Security/editions/2019/en/0xa6-mass-assignment/), focusing on the root cause: the lack of or improper authorization validation at the object property level. This leads to information exposure or manipulation by unauthorized parties."
[OWASP Top 10 API Security Risks – 2023 - OWASP API Security Top 10](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)

Reference: [API3:2023 - Broken Object Property Level Authorization](https://owasp.org/API-Security/editions/2023/en/0xa3-broken-object-property-level-authorization/)

In our two (2) labs, you worked on a BOLA. This is very similar but in this case we are attempting to access the properties of an object (contain data that describes the attributes of the object). These properties may contain data like email addresses, whether a user is an admin, social security numbers, bank account numbers, credit card numbers, healthcare information and more. In these labs we will look at vulnerabilities that allow us to access the properties directly without authorization.

## Discover other user's data (excessive data exposure)

Think back to when we might have already seen this failure? Yes, the "Community" forums page. Each post only displayed a few properties but the response included ALL of the properties. We will not do an additional lab here as you have already seen this one. Challenge completed.

## Discover the internal properties of a video

For this second example, we are going to explore the properties of the users video. We can upload a video to the profile and then look at the properties of the object. We will see that the application is providing us with information that is only needed internally for the server, not for the client browser.

#### Lab Steps

1. Using the Burp Suite Browser, click on the user profile icon.

2. The screen should update with information about your account. Near the middle of the page, there is a "My Personal Video" section. On the right of that line are three (3) vertical dots.

3. Click on the three (3) vertical dots and then select upload video.

   ![image-20240506223829668](C:\Users\Thomas.Freeman\AppData\Roaming\Typora\typora-user-images\image-20240506223829668.png)

4. If you then go back to the Proxy HTTP History window and search for the POST request URL, /identity/api/v2/user/videos, you will see information about the video in the response window.

   ![image-20240506225352300](C:\Users\Thomas.Freeman\AppData\Roaming\Typora\typora-user-images\image-20240506225352300.png)

5. The information includes internal the "conversion_params" object property. 

6. Challenge completed.

## Purchase an item for free (mass assignment)

For this challenge we will combine a business logic flaw and a mass assignment flaw to purchase an item, then mark the item as returned causing the system to refund the purchase amount. This allows us to get the product for free.

#### Lab Steps

1. Browse to the "Shop" menu to see the items for sale.

2. Notice you have a balance that allows you to buy something so go ahead and click on the "Buy" button next to one of the items.

3. Now switch back to the Proxy HTTP History window and look at the row with the URL POST request /workshop/api/shop/orders. Look at the request and the response to see the JSON parameters related to the order.

4. Switch back to the browser and click on the "Order Details" button.

5. Go to the Proxy HTTP History window and look at the row with the URL request /workshop/api/shop/orders/6. (Note: the ID is an integer. This would be a great candidate to send to Intruder and check other integers to see if you can get unauthorized access to other user's orders.)

6. Look at the Response Header and notice the HTTP methods that are allowed.

   ![image-20240506230600597](C:\Users\Thomas.Freeman\AppData\Roaming\Typora\typora-user-images\image-20240506230600597.png)

7. Let's right-click on the request and send it to repeater. Look back at previous labs if necessary to remember how to do this. 

8. Click on the repeater tab and you should see the request ready to be tweaked.

9. Next change the HTTP Method from "GET" to "PUT" (which is used when modifying an object in REST APIs). Add a simple JSON payload that updates the "status" to "returned". Then click on "Send".

   ![image-20240507151648936](C:\Users\Thomas.Freeman\AppData\Roaming\Typora\typora-user-images\image-20240507151648936.png)

10. If everything goes right you will see the status updated and a 200 success code.

11. Now browse back to the shop page and notice that your available balance is back to where you started. You ordered the part for free.

12. Challenge completed.
