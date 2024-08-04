# Brute-Forcing Login with Burp Suite on Parrot OS

## Why Use Burp Suite?
Burp Suite is a powerful tool for web application security testing. It allows you to:
- Intercept and modify HTTP requests and responses
- Perform automated scans for vulnerabilities
- Conduct various manual testing techniques
- Use advanced tools like Intruder for customized attacks, such as brute-forcing login credentials

Its flexibility and comprehensive feature set make it an essential tool for security professionals and penetration testers.

## Scenario
We'll be working with the "Alfred" machine on TryHackMe, which has a web server running on port 8080. Our goal is to brute-force the login page using Burp Suite on Parrot OS.

## Prerequisites
- Burp Suite installed on Parrot OS
- Web server running on port 8080

## Steps

### 1. Visit the Web Server
First, navigate to the web server running on port 8080. You should see a login page.

![Login Page](https://i.imgur.com/654aHM4.png)

### 2. Set Up Burp Suite Proxy
Ensure your browser is configured to use Burp Suite as a proxy. This setup allows Burp Suite to intercept HTTP requests between your browser and the web server, which is essential for capturing login attempts and manipulating them.

### 3. Attempt to Login and Capture the Request
Attempt to login with any credentials (e.g., `admin`/`pass`). Burp Suite will intercept the request. You will see the captured request in the **Proxy** tab.

![Intercepted Request](https://i.imgur.com/eirwkEx.png)

**Why?**
Capturing the request allows you to see the exact data being sent to the server, including the login parameters. This data is crucial for setting up the brute-force attack.

### 4. Send to Intruder
Right-click on the intercepted request and select **Send to Intruder**.

![Send to Intruder](https://i.imgur.com/w9zJjwn.png)

**Why?**
The Intruder tool in Burp Suite is designed for automating customized attacks, such as brute-forcing login credentials. Sending the request to Intruder prepares it for further configuration.

### 5. Set Intruder Positions
Go to the **Intruder** tab and then the **Positions** sub-tab. Clear any existing positions by clicking **Clear §**. Highlight the password parameter value and click **Add §** to mark it.

![Intruder Positions](https://i.imgur.com/iNIX2pl.png)

**Why?**
Setting positions tells Intruder which part of the request to modify during the attack. By marking the password field, you instruct Intruder to cycle through different passwords while keeping other parts of the request constant.

### 6. Configure Payloads
Navigate to the **Payloads** tab. Set the **Payload set** to 1 and the **Payload type** to **Simple list**. 

![Insert](https://i.imgur.com/oMPqziU.png)

Download a simple password list from [SecLists](https://github.com/danielmiessler/SecLists/blob/master/Passwords/2023-200_most_used_passwords.txt) and save it as `passwords.txt`.

Click **Load** and select your `passwords.txt` file.

![Load Payloads](https://i.imgur.com/3CtC81S.png) 
![Load Payloads](https://i.imgur.com/j5YPeDS.png)
![blob:https://imgur.com/0682777b-b5b2-41fe-a90e-4c32764a58a4](https://i.imgur.com/0pEYAbF.png)

**Why?**
Payloads are the data that Intruder will use to replace the marked positions in the request. Loading a list of common passwords increases the likelihood of guessing the correct password if it is a weak or commonly used one.

### 7. Start the Attack
Leave everything else as default and click **Start attack**.


**Why?**
Starting the attack will initiate the brute-force process, where Burp Suite systematically tries each password from the list against the login form.

### 8. Analyze Results
Look at the attack results. The first attempt should show a login error. Look for any attempt that does not show this error.

![Attack Results](https://i.imgur.com/xoePKA7.png)

**Why?**
By analyzing the results, you can identify which login attempts were unsuccessful (showing login errors) and which ones might have been successful (no error message). This helps pinpoint the correct credentials.

### 9. Successful Login
The one attempt that doesnt have an error is  `admin`/`admin` use these credentials to log in.

![Successful Login](https://i.imgur.com/7FDrKZu.png)

![Attack Results](https://i.imgur.com/BuXJhFm.png)

![Attack Results](https://i.imgur.com/znHsVxN.png)

### 10. Answering the next question in the Alfred TryHackMe Room

We have the credentials and we can enter them in to the next question to get the correct answer

![Attack Results](https://i.imgur.com/8ZFfVvI.png)

### Conclusion
Using Burp Suite's Intruder tool, we successfully brute-forced the login credentials for the server. The correct username and password were `admin`/`admin`.

This guide provides a basic example of how to use Burp Suite for brute-forcing login pages. Always ensure you have permission before testing any web application.
