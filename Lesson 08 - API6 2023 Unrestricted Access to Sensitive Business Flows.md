## Lesson 08 - API6: 2023 Unrestricted Access to Sensitive Business Flows

"APIs vulnerable to this risk expose a business flow - such as buying a ticket, or posting a comment - without compensating for how the functionality could harm the business if used excessively in an automated manner. This doesn't necessarily come from implementation bugs."
[OWASP Top 10 API Security Risks – 2023 - OWASP API Security Top 10](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)

Reference: [API6:2023 - Unrestricted Access to Sensitive Business Flows](https://owasp.org/API-Security/editions/2023/en/0xa6-unrestricted-access-to-sensitive-business-flows/)

Sensitive business flows may be modeled through Object properties and methods and thus may overlap with the earlier vulnerabilities we have discussed. The issue with business flows, is they are logical vulnerabilities. Meaning the system works as planned but if you do something out of order or in an unexpected manner, the business flow may result in unwanted results. Like marking an item returned when it was never received.

In a sense, these are abuses of the system. System functions that work they way the developer wanted them may be abused by clever attackers.

## Purchase an item at no cost

We completed this challenge back when working through the Broken Object Property Level Authorization (BPLA) labs because it used a combination of these two vulnerabilities to complete it.

## Increase account balance to more than $1000

If you were thinking about it, when we completed the previous challenge, we only changed the "status" to returned and this zeroed out our costs. But were there other parameters that we could have changed as well? It turns out that we can also change the quantity of items purchased. So if we purchase one item and have $10 deducted from our account but then modify the order to show we purchased 100 items and returned them, we should get $1000 added to our account. Let's give it a try.

#### Lab Steps

1. Browse to the "Shop" menu to see the items for sale.

2. Notice you have a balance that allows you to buy something so go ahead and click on the "Buy" button next to one of the items.

3. Now switch back to the Proxy HTTP History window and look at the row with the URL POST request /workshop/api/shop/orders. Look at the request and the response to see the JSON parameters related to the order.

4. Switch back to the browser and click on the "Order Details" button.

5. Go to the Proxy HTTP History window and look at the row with the URL request /workshop/api/shop/orders/6. (Note: the ID is an integer. This would be a great candidate to send to Intruder and check other integers to see if you can get unauthorized access to other user's orders.)

6. Look at the Response Header and notice the HTTP methods that are allowed.

   ![image-20240506230600597](file:///C:/Users/Thomas.Freeman/AppData/Roaming/Typora/typora-user-images/image-20240506230600597.png?lastModify=1715108116)

7. Let's right-click on the request and send it to repeater. Look back at previous labs if necessary to remember how to do this. 

8. Click on the repeater tab and you should see the request ready to be tweaked.

9. Next change the HTTP Method from "GET" to "PUT" (which is used when modifying an object in REST APIs). Add a simple JSON payload that updates the "status" to "returned" and the "quantity" to 100. Then click on "Send".

   ![image-20240507141715413](C:\Users\Thomas.Freeman\AppData\Roaming\Typora\typora-user-images\image-20240507141715413.png)

10. If everything goes right you will see the status updated and a 200 success code.

11. Now browse back to the shop page and notice that your available balance has increased by $1000. You made money on that order!

12. Challenge completed.

## Change the internal properties for a video

For this second example, we are going to explore the properties of the users video. We can upload a video to the profile and then look at the properties of the object. We will see that the application is providing us with information that is only needed internally for the server, not for the client browser.

#### Lab Steps

1. Using the Burp Suite Proxy HTTP History window, search for the POST request URL, /identity/api/v2/user/videos/(some number), you will see information about the video in the response window.

   ![image-20240507164527804](C:\Users\Thomas.Freeman\AppData\Roaming\Typora\typora-user-images\image-20240507164527804.png)

2. The information includes internal the "conversion_params" object property. Lets see if we can craft a request that will modify the parameter.

3. Right-click on the Request and send it to Intruder.

4. Add a conversion parameter of of webm.

   ![image-20240507165108589](C:\Users\Thomas.Freeman\AppData\Roaming\Typora\typora-user-images\image-20240507165108589.png)

5. Challenge completed.
