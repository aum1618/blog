
---
### Lab: [Title Here]
[Link to Lab]()

#apprentice
#### Instructions
This lab contains a [[SQL injection]] vulnerability in the product category filter. When the user selects a category, the application carries out a SQL query like the following:
To solve the lab, perform a SQL injection attack that causes the application to display one or more unreleased products.

#### Solution
1. Use Burp Suite to intercept and modify the request that sets the product category filter.
2. Modify the `category` parameter, giving it the value `'+OR+1=1--`
3. Submit the request, and verify that the response now contains one or more unreleased products.

---
