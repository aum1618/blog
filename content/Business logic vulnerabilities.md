---
tags:
  - portSwiggerAcademy
  - web
  - serverSide
  - ctf
---
[Link to the Lesson](https://portswigger.net/web-security/logic-flaws)
**Number of Labs:** 11

---
Basically these are flaws in the design and implementation of an application that may allow user to access information that they should not have access to and in return causing them to harm the system. 

>[!NOTE] NOTE
In this context, the term "business logic" simply refers to the set of rules that define how the application operates. As these rules aren't always directly related to a business, the associated vulnerabilities are also known as "application logic vulnerabilities" or simply "logic flaws".

## LABS

---
### Lab: Excessive trust in client-side controls
This lab doesn't adequately validate user input. You can exploit a logic flaw in its purchasing workflow to buy items for an unintended price. To solve the lab, buy a "Lightweight l33t leather jacket".

#### Solution
1. With Burp running, log in and attempt to buy the leather jacket. The order is rejected because you don't have enough store credit.
2. In Burp, go to "Proxy" > "HTTP history" and study the order process. Notice that when you add an item to your cart, the corresponding request contains a `price` parameter. Send the `POST /cart` request to Burp Repeater.
3. In Burp Repeater, change the price to an arbitrary integer and send the request. Refresh the cart and confirm that the price has changed based on your input.
4. Repeat this process to set the price to any amount less than your available store credit.
5. Complete the order to solve the lab.

---
### Lab: 2FA Broken Logic
This lab's two-factor authentication is vulnerable due to its flawed logic. To solve the lab, access Carlos's account page.
- Your credentials: `wiener:peter`
- Victim's username: `carlos`
You also have access to the email server to receive your 2FA verification code.
#### Solution
1. With Burp running, log in to your own account and investigate the 2FA verification process. Notice that in the `POST /login2` request, the `verify` parameter is used to determine which user's account is being accessed.
2. Log out of your account.
3. Send the `GET /login2` request to Burp Repeater. Change the value of the `verify` parameter to `carlos` and send the request. This ensures that a temporary 2FA code is generated for Carlos.
4. Go to the login page and enter your username and password. Then, submit an invalid 2FA code.
5. Send the `POST /login2` request to Burp Intruder.
6. In Burp Intruder, set the `verify` parameter to `carlos` and add a payload position to the `mfa-code` parameter. Brute-force the verification code.
7. Load the 302 response in the browser.
8. Click **My account** to solve the lab.

---
### Lab: High-level logic vulnerability

This lab doesn't adequately validate user input. You can exploit a logic flaw in its purchasing workflow to buy items for an unintended price. To solve the lab, buy a "Lightweight l33t leather jacket".

You can log in to your own account using the following credentials: `wiener:peter`

#### Solution
1. With Burp running, log in and add a cheap item to your cart.
2. In Burp, go to "Proxy" > "HTTP history" and study the corresponding HTTP messages. Notice that the quantity is determined by a parameter in the `POST /cart` request.
3. Go to the "Intercept" tab and turn on interception. Add another item to your cart and go to the intercepted `POST /cart` request in Burp.
4. Change the `quantity` parameter to an arbitrary integer, then forward any remaining requests. Observe that the quantity in the cart was successfully updated based on your input.
5. Repeat this process, but request a negative quantity this time. Check that this is successfully deducted from the cart quantity.
6. Request a suitable negative quantity to remove more units from the cart than it currently contains. Confirm that you have successfully forced the cart to contain a negative quantity of the product. Go to your cart and notice that the total price is now also a negative amount.
7. Add the leather jacket to your cart as normal. Add a suitable negative quantity of the another item to reduce the total price to less than your remaining store credit.
8. Place the order to solve the lab.

---

### Lab: Low-level logic flaw

This lab doesn't adequately validate user input. You can exploit a logic flaw in its purchasing workflow to buy items for an unintended price. To solve the lab, buy a "Lightweight l33t leather jacket".

You can log in to your own account using the following credentials: `wiener:peter`

---
### Lab: Inconsistent handling of exceptional input

This lab doesn't adequately validate user input. You can exploit a logic flaw in its account registration process to gain access to administrative functionality. To solve the lab, access the admin panel and delete the user `carlos`.

#### Solution
1. While proxying traffic through Burp, open the lab and go to the "Target" > "Site map" tab. Right-click on the lab domain and select "Engagement tools" > "Discover content" to open the content discovery tool.
2. Click "Session is not running" to start the content discovery. After a short while, look at the "Site map" tab in the dialog. Notice that it discovered the path `/admin`.
3. Try to browse to `/admin`. Although you don't have access, an error message indicates that `DontWannaCry` users do.
4. Go to the account registration page. Notice the message telling `DontWannaCry` employees to use their company email address.
5. From the button in the lab banner, open the email client. Make a note of the unique ID in the domain name for your email server (`@YOUR-EMAIL-ID.web-security-academy.net`).
6. Go back to the lab and register with an exceptionally long email address in the format:
    
    `very-long-string@YOUR-EMAIL-ID.web-security-academy.net`
    
    The `very-long-string` should be at least 200 characters long.
    
7. Go to the email client and notice that you have received a confirmation email. Click the link to complete the registration process.
8. Log in and go to the "My account" page. Notice that your email address has been truncated to 255 characters.
9. Log out and go back to the account registration page.
10. Register a new account with another long email address, but this time include `dontwannacry.com` as a subdomain in your email address as follows:
    
    `very-long-string@dontwannacry.com.YOUR-EMAIL-ID.web-security-academy.net`
    
    Make sure that the `very-long-string` is the right number of characters so that the "`m`" at the end of `@dontwannacry.com` is character 255 exactly.
    
11. Go to the email client and click the link in the confirmation email that you have received. Log in to your new account and notice that you now have access to the admin panel. The confirmation email was successfully sent to your email client, but the application server truncated the address associated with your account to 255 characters. As a result, you have been able to register with what appears to be a valid `@dontwannacry.com` address. You can confirm this from the "My account" page.
12. Go to the admin panel and delete `carlos` to solve the lab.

---
### Lab: Flawed enforcement of business rules

This lab has a logic flaw in its purchasing workflow. To solve the lab, exploit this flaw to buy a "Lightweight l33t leather jacket".

You can log in to your own account using the following credentials: `wiener:peter`

#### Solution
1. Log in and notice that there is a coupon code, `NEWCUST5`.
2. At the bottom of the page, sign up to the newsletter. You receive another coupon code, `SIGNUP30`.
3. Add the leather jacket to your cart.
4. Go to the checkout and apply both of the coupon codes to get a discount on your order.
5. Try applying the codes more than once. Notice that if you enter the same code twice in a row, it is rejected because the coupon has already been applied. However, if you alternate between the two codes, you can bypass this control.
6. Reuse the two codes enough times to reduce your order total to less than your remaining store credit. Complete the order to solve the lab.

---
### Lab: Infinite money logic flaw

This lab has a logic flaw in its purchasing workflow. To solve the lab, exploit this flaw to buy a "Lightweight l33t leather jacket".

You can log in to your own account using the following credentials: `wiener:peter`

#### Solution
1. With Burp running, log in and sign up for the newsletter to obtain a coupon code, `SIGNUP30`. Notice that you can buy $10 gift cards and redeem them from the "My account" page.
2. Add a gift card to your basket and proceed to the checkout. Apply the coupon code to get a 30% discount. Complete the order and copy the gift card code to your clipboard.
3. Go to your account page and redeem the gift card. Observe that this entire process has added $3 to your store credit. Now you need to try and automate this process.
4. Study the proxy history and notice that you redeem your gift card by supplying the code in the `gift-card` parameter of the `POST /gift-card` request.
5. Go to "Project options" > "Sessions". In the "Session handling rules" panel, click "Add". The "Session handling rule editor" dialog opens.
6. In the dialog, go to the "Scope" tab. Under "URL Scope", select "Include all URLs".
7. Go back to the "Details" tab. Under "Rule actions", click "Add" > "Run a macro". Under "Select macro", click "Add" again to open the Macro Recorder.
8. Select the following sequence of requests:
    
    `POST /cart POST /cart/coupon POST /cart/checkout GET /cart/order-confirmation?order-confirmed=true POST /gift-card`
    
    Then, click "OK". The Macro Editor opens.
    
9. In the list of requests, select `GET /cart/order-confirmation?order-confirmed=true`. Click "Configure item". In the dialog that opens, click "Add" to create a custom parameter. Name the parameter `gift-card` and highlight the gift card code at the bottom of the response. Click "OK" twice to go back to the Macro Editor.
10. Select the `POST /gift-card` request and click "Configure item" again. In the "Parameter handling" section, use the drop-down menus to specify that the `gift-card` parameter should be derived from the prior response (response 4). Click "OK".
11. In the Macro Editor, click "Test macro". Look at the response to `GET /cart/order-confirmation?order-confirmation=true` and note the gift card code that was generated. Look at the `POST /gift-card` request. Make sure that the `gift-card` parameter matches and confirm that it received a `302` response. Keep clicking "OK" until you get back to the main Burp window.
12. Send the `GET /my-account` request to Burp Intruder. Use the "Sniper" attack type.
13. On the "Payloads" tab, select the payload type "Null payloads". Under "Payload settings", choose to generate `412` payloads.
14. Go to the "Resource pool" tab and add the attack to a resource pool with the "Maximum concurrent requests" set to `1`. Start the attack.
15. When the attack finishes, you will have enough store credit to buy the jacket and solve the lab.