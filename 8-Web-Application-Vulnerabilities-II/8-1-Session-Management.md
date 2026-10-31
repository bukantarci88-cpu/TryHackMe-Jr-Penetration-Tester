# Jr. Pent  8.1. Session Management

### **Task 1 - Introduction**

Session management securely maintains user state and authorization after authentication to prevent session hijacking.

### Task 2 - What is Session Management?

HTTP's stateless nature means sessions are essential for tracking users securely across requests.

**Session Management Lifecycle**

**Session Creation:** Initializes a session (either pre-authentication for tracking or post-authentication for secure access).

**Session Tracking:** Sends the session value with each request to bridge HTTP's stateless nature and identify the user.

**Session Expiry:** Enforces a session lifetime so abandoned or inactive sessions automatically become invalid.

**Session Termination:** Explicitly ends a session when the user logs out, preventing persistent access even if the session's lifetime hasn't expired.

**Answer the questions below**

> Which state in the session management lifecycle deals with user's pressing the logout button?
> 

Session Termination

> Which state in the session management lifecycle deals with user's providing their credentials?
> 

Session Creation

> Which state in the session management lifecycle deals with user's actions performed after authentication?
> 

Session Tracking

> Which state in the session management lifecycle deals with user's that forget to log out of the application?
> 

Session Expiry

### Task 3 - Authentication vs Authorisation

![IAAA Model](https://cdn-images.tryhackme.com/user-uploads/6093e17fa004d20049b6933e/room-content/6093e17fa004d20049b6933e-1722529402239.png)

**Identification:** Identification is the act of a user claiming a specific identity, usually by providing a username or email address.

**Authentication:** Authentication verifies the claimed identity through proof, such as a password, which then triggers session creation.

**Authorisation:** Authorisation uses session tracking to check whether a verified user has permission to perform a requested action.

**Accountability:** Accountability involves logging user sessions and actions to maintain a record for security incident investigations.

**IAAA and Session Management:** Session management links IAAA together by creating sessions upon authentication, enforcing authorization during tracking, and logging sessions for accountability.

**Answer the questions below**

> What is the name of the process in the IAAA model that would be responsible for tracking your actions and logging them?
> 

Accountability

> What is the name of the process in the IAAA model that would be responsible for granting you a session value?
> 

Authentication

> What is the name of the process in the IAAA model that would be responsible for verifying that you have the relevant permissions to perform an action?
> 

Authorisation

### Task 4 - Cookies vs Tokens

**Cookie-Based Session Management**

Cookie-based session management is often called the old-school way of managing sessions. Once the web application wants to begin tracking, in a response, the Set-Cookie header value will be sent. `Set-Cookie: session=12345;`

Your browser will create a cookie entry for a cookie named `session` with a value of `12345` which will be valid for the domain where the cookie was received from.  

- **Secure** - Indicates to the browser that the cookie may only be transmitted over verified HTTPS channels. If there are certificate errors or HTTP is used, the cookie value will not be transmitted.
- **HTTPOnly** - Indicates to the browser that the cookie value may not be read by client-side JavaScript.
- **Expire** - Indicates to the browser when a cookie value will no longer be valid and should be removed.
- **SameSite** - Indicates to the browser whether the cookie may be transmitted in cross-site requests to help protect against CSRF attacks.

Browsers automatically attach cookies to requests based on their domain and attributes, eliminating the need for client-side JavaScript.

**Token-Based Session Management**

Token-based session management is a relatively new concept. Instead of using the browser's automatic cookie management features, it relies on client-side code for the process. After authentication, the web application provides a token within the request body. Using client-side JavaScript code, this token is then stored in the browser's LocalStorage.

When a new request is made, JavaScript code must load the token from storage and attach it as a header. One of the most common types of tokens is JSON Web Tokens (JWT), which are passed through the `Authorization: Bearer` header. However, as we are not using the browser's built-in cookie management features, it is a bit of the wild west where anything goes. Although there are standards, nothing is really forcing anything from sticking to these standards.

**Benefits and Drawbacks**

| **Cookie-Session Management** | **Token-Based Session Management** |
| --- | --- |
| Cookie is automatically sent by the browser with each request | Token has to be submitted as a header with each request using client-side JavaScript |
| Cookie attributes can be used to enhance the browser's protection of the cookie | Tokens do not have automatic security protections enforced and should, therefore, be safeguarded against disclosures |
| Cookies can be vulnerable to conventional client-side attacks such as CSRF, where the browser is tricked into making a request on behalf of the user. | As the token is not automatically added to any request and cannot be read from LocalStorage by other domains, conventional client-side attacks such as CSRF are blocked. |
| As cookies are locked to a specific domain, it can be difficult to use them securely in decentralised web applications. | Tokens work well in decentralised web applications, as they are managed through JavaScript and can often contain all the information required to verify the token itself. |

**Answer the questions below**

> What cookie attribute can be used to ensure that the cookie is only transmitted via secure HTTPS channels?
> 

secure

> What HTTP header is used in the response to inform the browser that a cookie is being sent?
> 

Set-Cookie

> What HTTP header is often used in requests to indicate the transmission of a JWT?
> 

Authorization: Bearer

### Task 5 - Securing the Session Lifecycle

**A. Session Creation:** Session creation is where the most vulnerabilities can creep in.  

**Weak Session Values:** It is less common to see weak session values in modern times as frameworks are consistently used. However, with the rise of LLMs and other AI code-assistant solutions, you would be surprised at how often these old-school vulnerabilities are creeping back in. If a custom session creation mechanism has been implemented, there is a good chance that the session values may be guessable. A good example of this is a mechanism that simply base64 encodes the username as the session value. If a threat actor can reverse engineer the session creation process, they can generate or guess session values to hijack the accounts of legitimate users.

**Controllable Session Values:** In certain tokens, such as JWTs, all the relevant information to both create and verify the JWT's validity is provided. If security measures are not enforced, such as verifying the token's signature or ensuring that the signature itself was created securely, a threat actor would be able to generate their own token. These types of attacks will be discussed in more detail in a future room.

**Session Fixation:** Failing to rotate session IDs after authentication lets attackers hijack pre-recorded unauthenticated sessions.

**Insecure Session Transmission:** Flaws in transferring session material between servers can expose session information to threat actors.

**B. Session Tracking**

**Authorisation Bypass:** Authorisation bypasses occur when there aren't sufficient checks being performed on whether a user is allowed to perform the action they requested. In essence, this fails to track the user's session and its associated rights correctly. It is also worth talking about the two types of authorisation bypasses:

- Vertical bypass - You can perform an action that is reserved for a more privileged user
- Horizontal bypass - You can perform an action you are allowed to perform, but on a dataset that you should not be allowed to perform the action on

In most applications, vertical bypasses are easy to defend against since function decorators and path-based access control configurations are used. However, with horizontal bypasses, the user is performing an action that they should be allowed to perform. The issue is that they are performing it on someone else's data. To remedy this, actual code is required to verify who the user is (extracted from their session), which data they are requesting, and if they are allowed to request or modify the dataset.

**Insufficient Logging:** Lacking detailed application logs that link sessions to user actions makes investigating security incidents difficult.

**Session Expiry:** Setting excessively long session lifetimes increases the window of opportunity for attackers to misuse active sessions.

**Session Termination:** Failing to properly invalidate sessions server-side during logout or password resets leaves accounts vulnerable to ongoing access.

**Answer the questions below**

> What phase of the session management lifecycle would be vulnerable if you could predict what the value of a session would be?
> 

Session Creation

> What phase of the session management lifecycle would be vulnerable if you don't have the ability to flush a threat actor's access to your session?
> 

Session Termination

> What phase of the session management lifecycle would be vulnerable if there wasn't sufficient information to piece together what happened during an incident?
> 

Session Tracking

> What phase of the session management lifecycle would be vulnerable if the session value itself was transmitted through an insecure redirect?
> 

Session Creation

### Task 6 - Exploiting Insecure Session Management

**Enumeration:** To effectively exploit a vulnerable session management lifecycle, we first need to map out the lifecycle for ourselves and capture detailed notes. Once we understand the intended lifecycle, we can start looking for weaknesses. We will be using the built-in browser tools for this demonstration. However, you can also follow along with more advanced web application testing tools, such as Burp. Let's navigate to our application at [http://MACHINE_IP](http://machine_ip/) first, and we should see the following page:

![Login Panel](https://cdn-images.tryhackme.com/user-uploads/6093e17fa004d20049b6933e/room-content/6093e17fa004d20049b6933e-1722450848675.png)

We first notice that we are not presented with any cookies or tokens when we first visit the page. This tells us that unauthenticated sessions are most likely not being tracked. If we click the Sign-Up button, we see that there are two main signups:

- Student - This can be used by anyone to create a profile
- Lecturer - Requires a verification code for the signup process

This shows us that even without any brute-force techniques, we can kick off the session management lifecycle by creating a student account. Let's create a student account and see what happens:

![Registration panel](https://cdn-images.tryhackme.com/user-uploads/6093e17fa004d20049b6933e/room-content/6093e17fa004d20049b6933e-1722451429592.png)

After creating the user account, we receive a prompt that the account has been created, and we are navigated back to the login page. Let's perform a login as our user and monitor the network traffic:

![Login Request](https://cdn-images.tryhackme.com/user-uploads/6093e17fa004d20049b6933e/room-content/6093e17fa004d20049b6933e-1722451429583.png)

This tells us quite a bit of information:

- Cookies are being used for session management.
- The HTTPOnly flag is set which means we would not be able to leverage JavaScript to read the cookie value.

With this authentication, we also notice that we now have access to more functionality:

![Dashboard view](https://cdn-images.tryhackme.com/user-uploads/6093e17fa004d20049b6933e/room-content/6093e17fa004d20049b6933e-1722451873193.png)

Using the menu navigation and reviewing the network traffic, we can see that session tracking is enforced through the cookie that is being transmitted with each request:

![Modules request](https://cdn-images.tryhackme.com/user-uploads/6093e17fa004d20049b6933e/room-content/6093e17fa004d20049b6933e-1722451873192.png)

Note that your headers might differ slightly depending on which browser you use. However, an interesting thing to note is that even though the cookie expiry time has not yet been reached, the same set cookie header is used to refresh the cookie to the exact same value and extend the lifetime even further. This might point to a persistent cookie. Let's see what happens when we remove this cookie. Under storage, remove the cookie and make the module request again:

![Modules request](https://cdn-images.tryhackme.com/user-uploads/6093e17fa004d20049b6933e/room-content/6093e17fa004d20049b6933e-1722452082078.png)

The request seems to still succeed? However, if you take a closer look, you will see that we can now only see the modules but not the number of enrolled students or the potential tests:

![Dashboard view](https://cdn-images.tryhackme.com/user-uploads/6093e17fa004d20049b6933e/room-content/6093e17fa004d20049b6933e-1722452094296.png)

This tells us that we might need to do further investigation to determine exactly what we are allowed to access from a completely unauthenticated perspective. This would warrant further investigation to fully map out the session management lifecycle. However, we will keep things slightly simpler. Let's take a look at the logout functionality. Once we press the logout button, we can see that our session is removed client-side:

![Cookie cache](https://cdn-images.tryhackme.com/user-uploads/6093e17fa004d20049b6933e/room-content/6093e17fa004d20049b6933e-1722452255723.png)

However, we should probably test if the session is also terminated server-side. You can re-authenticate to the application and replace your new cookie with your old cookie value. But, it does seem like the session termination is working to some capacity as you get a 500 Internal Server Error when you refresh the page. This does tell us that there is something wrong here, since an invalid session should not lead to an internal server error. Let's investigate.

Let's revisit some of our previous claims here. If we navigate to the Local Storage, we do notice quite a bit of data is being stored:

![Token cache](https://cdn-images.tryhackme.com/user-uploads/6093e17fa004d20049b6933e/room-content/6093e17fa004d20049b6933e-1722454261803.png)

Let's play around by updating our userRole from student to lecturer and refreshing the page:

![Dashboard view](https://cdn-images.tryhackme.com/user-uploads/6093e17fa004d20049b6933e/room-content/6093e17fa004d20049b6933e-1722454362144.png)

While we now have access to more tabs, it seems like we don't have access to more information since we can see any students or test attempts. Let's take this further by updating our role in the user value from a 2 to a 3. Sadly, this still gives us the same results. However, there are more values to play with here, such as our id and username. We will have to come back to this. However, this new information tells us that the sessions might be managed not purely through a cookie but through token information.

Now, we have mapped out the session management lifecycle. Let's take a look:

| **Lifecycle Phase** | **Observation** |
| --- | --- |
| Session Creation | A combination of cookie- and token-based session management is being used |
| Session Creation | A new session value is provided for each login attempt |
| Session Creation | The session value itself appears to be sufficiently random |
| Session Tracking | Unauthenticated actions are not tracked via a session |
| Session Tracking | The cookie is sent in each request for tracking |
| Session Tracking | Several requests can be made without a cookie |
| Session Tracking | The initial token values that are loaded post-authentication dictate the visible information |
| Session Expiry | There is a mismatch between the client-side and server-side expiry times |
| Session Expiry | The expiry time is incredibly long |
| Session Termination | Users can forcibly terminate their session |
| Session Termination | Sessions are terminated server-side but reusing an old session leads to an internal server error |

Given all of this information, we would already report the following vulnerabilities on a security assessment:

- Excessive Session Lifetimes

We would also need to investigate the following further:

- Access controls on all API endpoints
- Web application logs for the accountability of actions

**Answer the questions below**

> What is the username of the student with the name X?
> 

THM{Got.the.User}

Register on the page, log in, then log out and log in again. At the same time, open an Incognito tab in Firefox. Go to Inspect Element, navigate to Storage, and set the user role to 'lecturer'.


<img width="1202" height="804" alt="image" src="https://github.com/user-attachments/assets/c4d310b1-1b20-4a9b-b8b3-e41903192da9" />


> How many lecturers are registered on the application?
> 

1

Set cooky for admin


<img width="1202" height="804" alt="image 1" src="https://github.com/user-attachments/assets/1df94949-576f-453a-a2d8-368179cc5d2c" />


> Excluding the unauthenticated user, how many roles does the application have?
> 

3

student, lecturer, admin

> How many test attempts in total have been performed on the application?
> 

4


<img width="1202" height="510" alt="image 2" src="https://github.com/user-attachments/assets/5590eb48-6593-4369-bb10-d745bbf86b09" />


> What is the highest score that student1 has achieved on a test?
> 

3 

Go to the Test section on the page. Select the database types, then click 'View’

> What is the sequence of correct answers for the Database Types test?
> 

y,n,n

"Go to the **Test** section on the page, select **Database Types**, and click **View**.

### Task 7 - Conclusion

To defend against these attacks, it is important to implement a secure session management lifecycle.  

- The session's values must be stored securely, regardless of being a cookie or a token.
- The session values themselves must be either sufficiently random and non-guessable or use a signing mechanism to ensure that they cannot be tampered with.
- Sessions should be used to track user actions and perform authorisation checks to ensure the user can perform the requested action.
- Sessions should expire after a set amount of time to prevent them from being used for persistent access.
- If the logout button is pressed, the session should be removed client-side and invalidated server-side. Otherwise, a user would be unable to destroy their session if it was compromised.
