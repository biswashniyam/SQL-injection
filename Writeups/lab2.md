# This lab contains a SQL injection vulnerability in the login function.
## To solve the lab, perform a SQL injection attack that logs in to the application as the administrator user.

## Step1....Visit login page
![access](Images/L2step1.png)

## step 2....login using user id: administrator.
![login](Images/L2step2.png)

- this displays a incorrect id or password.

## step 3: Let’s craft a payload to bypass this login as administrator.
- we intercept request of login using burpsuite and inject SQL query which directly login as admin.
- normal request looks like this:
![normal_request](Images/L2step3.png)

## step 4 :modify username
```
administrator' --
```
![modified_request](Images/L2step4.png)

- this will directly login as a administrator.

## lab solved
![lab_solved](Images/L2step5.png)

