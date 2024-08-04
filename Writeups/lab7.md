# Lab 7

# This lab contains a SQL injection vulnerability in the product category filter.

- The results from the query are returned in the application's response, so you can use a UNION attack   to retrieve data from other tables.
- The first step of such an attack is to *determine the number of columns* that are being returned by the query.

# To solve the lab, determine the number of columns returned by the query by performing a SQL injection UNION attack that returns an additional row containing null values.

# Step 1 : acess the lab

# step 2 : Determine the no of column returned by a query
- There are two effective methods to determine how many columns are being returned from the original query.
- ORDER BY
```
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
```

- UNION SELECT
```SQL
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
```
- If the number of nulls does not match the number of columns, the database returns an error.

# Use Burp Suite to intercept and modify the request that sets the product category filter.

# step 3 : Modify product category parameter
- Modify the category parameter, giving it the value.
```SQL
'+UNION+SELECT+NULL--
```

- this shows internal server error.
- Modify the category parameter to add an additional column containing a null value:
```
'+UNION+SELECT+NULL,NULL--
```
- add null values until the eroor disappears.

# final solution
```SQL
'+UNION+SELECT+NULL,NULL,NULL--
```

![lab_solved](Images/L4step1.png)





