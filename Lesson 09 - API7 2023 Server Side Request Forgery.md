## Lesson 09 - API7: 2023 Server Side Request Forgery

"Server-Side Request Forgery (SSRF) flaws can occur when an API is fetching a remote resource without validating the user-supplied URI. This enables an attacker to coerce the application to send a crafted request to an unexpected destination, even when protected by a firewall or a VPN."
[OWASP Top 10 API Security Risks – 2023 - OWASP API Security Top 10](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)

Reference: [API7:2023 - Server Side Request Forgery](https://owasp.org/API-Security/editions/2023/en/0xa7-server-side-request-forgery/)

SSRF vulnerabilities allow the attacker to have the server make requests to other websites. Some internet resources use allow lists to limit the IP addresses that can connect to them. For instance, a website might only allow connections from the IP address of the webserver. If we can get the webserver to make a request for us, it will typically be accepted.

## Force the API Service to make a request to SummitCreditUnion.com

In this example, we noticed earlier that the "Contact Mechanic" request sent a hardcoded URL. If we repeat that request but change the URL can we make the application send requests to other servers"? If we can, then we have, in a sense, proxied our traffic through their the crAPI application. And the website receiving the request will see it as coming from the application rather than from us. This could bypass an allow-list control.

#### Lab Steps

1. Browse to the "Dashboard" menu to see main page.

2. Click on the "Contact Mechanic" button. Fill in the form and click on the "Send Service Request".

3. Go to the Proxy HTTP History window and look for the POST request URL /workshop/api/merchant/contact_mechanic. Right-click on this row and send to Repeater.

4. Send the request and note the response.

5. Now change the "mechanic_api" parameter to be "http://ipinfo.io/ip" and send the request.

   ![image-20240507182722134](C:\Users\Thomas.Freeman\OneDrive - Sikich LLP\Documents\APISecTraining\appsec-train\Files\image-20240507182722134.png)

6. Notice that we have successfully sent a webquery to ipinfo.io and it has returned the IP address our request came from.

7. There is an additional API endpoint that will only present an error message unless the request originates from the application itself.

   ![image-20240507183013022](C:\Users\Thomas.Freeman\OneDrive - Sikich LLP\Documents\APISecTraining\appsec-train\Files\image-20240507183013022.png)

8. If you look at the capitalized letters in the response, you will note that they spell out S S R F. 

9. crAPI is vulnerable to an advanced attack where you setup a Netcat listener on a machine of your choice and then set the conversion codec to be a bash command to connect to your listener. You can then use the "contact_mechanic" endpoint to trigger the video conversion which executes the bash scripts and initiate a reverse shell back to your box allowing you to gain root access to the host system. This is a worse case scenario!

10. Challenge completed.

