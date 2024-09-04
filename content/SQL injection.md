---
title: SQL Injections
tags:
  - serverSide
  - portSwiggerAcademy
  - web
---
---

SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. This can allow an attacker to view data that they are not normally able to retrieve. This might include data that belongs to other users, or any other data that the application can access. In many cases, an attacker can modify or delete this data, causing persistent changes to the application's content or behavior.

## Detecting SQL Injections
- Looking for errors or anomalies by submitting abnormal inputs . For example a single `'` can be used to detect SQL Injections.
Let's say there is a normal query in the backend that looks like this:
```sql
Select * from items where type='USER_INPUT'
```
 but if the user input is a single ' than this input would become:

```sql
Select * from items where type='''
```
Which we can see would result in a syntax error and we might get the error in the response confirming that the user input is SQL vulnerable.

- You can also detect SQL by submitting some different value than the intended value and see the difference in responses.
Let's say you have an SQL query:
```sql
SELECT * FROM users WHERE username = '$username' AND password = '$password';
```
in this you can give an input like :
```
username=admin
password=penguin123
```
but instead you give the following value:

```
username=admin'--
password=penguin123
```
In this case the `--` will have a lot of affect as this will comment out all the SQL following it and resulting in an easy getaway and login without password.

```sql
SELECT * FROM users WHERE username = 'admin'--' AND password = '$password';
```

- You can also submit Boolean values like `1=1` and `1=2` that will evaluate to true or false respectively and you can see the difference between the responses to determine the presence of SQL Vulnerability
- You can also put in an SQL payload for a time based that will delay the request and you can observe delay in the responses time to determine presence of SQL vulnerability.

---
## Practical
- [[PortSwigger Labs SQL Injection]]
- 