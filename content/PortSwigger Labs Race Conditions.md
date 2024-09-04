---
title: "PortSwigger Labs: Race Conditions"
tags:
  - portSwiggerAcademy
  - writeUp
  - serverSide
  - web
---
[Link To Lesson](https://portswigger.net/web-security/race-conditions)
**Number of Labs:** 06 

---
### Lab: Limit overrun race conditions
#apprentice 
[Link To Lab](https://portswigger.net/web-security/race-conditions/lab-race-conditions-limit-overrun)

#### Instructions
This lab's purchasing flow contains a race condition that enables you to purchase items for an unintended price.

To solve the lab, successfully purchase a **Lightweight L33t Leather Jacket**.

You can log in to your account with the following credentials: `wiener:peter`.

#### Solution
##### Predict a potential collision

1. Log in and buy the cheapest item possible, making sure to use the provided discount code so that you can study the purchasing flow.
    
2. Consider that the shopping cart mechanism and, in particular, the restrictions that determine what you are allowed to order, are worth trying to bypass.
    
3. In Burp, from the proxy history, identify all endpoints that enable you to interact with the cart. For example, a `POST /cart` request adds items to the cart and a `POST /cart/coupon` request applies the discount code.
    
4. Try to identify any restrictions that are in place on these endpoints. For example, observe that if you try applying the discount code more than once, you receive a `Coupon already applied` response.
    
5. Make sure you have an item to your cart, then send the `GET /cart` request to Burp Repeater.
    
6. In Repeater, try sending the `GET /cart` request both with and without your session cookie. Confirm that without the session cookie, you can only access an empty cart. From this, you can infer that:
    
    - The state of the cart is stored server-side in your session.
    - Any operations on the cart are keyed on your session ID or the associated user ID.
    
    This indicates that there is potential for a collision.
    
7. Consider that there may be a race window between when you first apply a discount code and when the database is updated to reflect that you've done this already.
    

##### Benchmark the behavior

1. Make sure there is no discount code currently applied to your cart.
    
2. Send the request for applying the discount code (`POST /cart/coupon`) to Repeater.
    
3. In Repeater, add the new tab to a group. For details on how to do this, see [Creating a new tab group](https://portswigger.net/burp/documentation/desktop/tools/repeater/groups#creating-a-new-tab-group).
    
4. Right-click the grouped tab, then select **Duplicate tab**. Create 19 duplicate tabs. The new tabs are automatically added to the group.
5. Send the group of requests in sequence, using separate connections to reduce the chance of interference. For details on how to do this, see [Sending requests in sequence](https://portswigger.net/burp/documentation/desktop/tools/repeater/send-group#sending-requests-in-sequence).
    
6. Observe that the first response confirms that the discount was successfully applied, but the rest of the responses consistently reject the code with the same **Coupon already applied** message.
    

##### Probe for clues

1. Remove the discount code from your cart.
    
2. In Repeater, send the group of requests again, but this time in parallel, effectively applying the discount code multiple times at once. For details on how to do this, see [Sending requests in parallel](https://portswigger.net/burp/documentation/desktop/tools/repeater/send-group#sending-requests-in-parallel).
    
3. Study the responses and observe that multiple requests received a response indicating that the code was successfully applied. If not, remove the code from your cart and repeat the attack.
    
4. In the browser, refresh your cart and confirm that the 20% reduction has been applied more than once, resulting in a significantly cheaper order.
    

##### Prove the concept

1. Remove the applied codes and the arbitrary item from your cart and add the leather jacket to your cart instead.
    
2. Resend the group of `POST /cart/coupon` requests in parallel.
    
3. Refresh the cart and check the order total:
    
    - If the order total is still higher than your remaining store credit, remove the discount codes and repeat the attack.
    - If the order total is less than your remaining store credit, purchase the jacket to solve the lab.

---
