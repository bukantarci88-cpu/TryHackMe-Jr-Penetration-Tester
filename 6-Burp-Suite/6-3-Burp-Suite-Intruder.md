# Jr. Pen. 6.3. Burp Suite: Intruder

### **Task 1 - Introduction**

Burp Suite’s Intruder module, which offers automated request manipulation and enables tasks such as fuzzing and brute-forcing. Burp Suite’s Intruder module is a powerful tool that allows for automated and customisable attacks. It provides the ability to modify specific parts of a request and perform repetitive tests with variations of input data. Intruder is particularly useful for tasks like fuzzing and brute-forcing, where different values need to be tested against a target.

### **Task 2 - What is Intruder**

Intruder is Burp Suite’s built-in fuzzing tool that allows for automated request modification and repetitive testing with variations in input values. By using a captured request (often from the Proxy module), Intruder can send multiple requests with slightly altered values based on user-defined configurations. It serves various purposes, such as brute-forcing login forms by substituting username and password fields with values from a wordlist or performing fuzzing attacks using wordlists to test subdirectories, endpoints, or virtual hosts. Intruder’s functionality is comparable to command-line tools like **Wfuzz** or **ffuf**.

![image.png](Jr%20Pen%206%203%20Burp%20Suite%20Intruder/image.png)



There are four sub-tabs within Intruder:

**Positions:** This tab allows us to select an attack type (which we will cover in a future task) and configure where we want to insert our payloads in the request template.
**Payloads:** Here we can select values to insert into the positions defined in the Positions tab. We have various payload options, such as loading items from a wordlist. The way these payloads are inserted into the template depends on the attack type chosen in the Positions tab. The Payloads tab also enables us to modify Intruder’s behavior regarding payloads, such as defining pre-processing rules for each payload (e.g., adding a prefix or suffix, performing match and replace, or skipping payloads based on a defined regex).
**Resource Pool:** This tab is not particularly useful in the Burp Community Edition. It allows for resource allocation among various automated tasks in Burp Professional. Without access to these automated tasks, this tab is of limited importance.
**Settings:** This tab allows us to configure attack behavior. It primarily deals with how Burp handles results and the attack itself. For instance, we can flag requests containing specific text or define Burp’s response to redirect (3xx) responses.

> In which Intruder tab can we define the "Attack type" for our planned attack? Positions
> 

### **Task 3 - Positions**

When using Burp Suite Intruder to perform an attack, the first step is to examine the positions within the request where we want to insert our payloads. 

![image.png](Jr%20Pen%206%203%20Burp%20Suite%20Intruder/image%201.png)

Notice that Burp Suite automatically attempts to identify the most probable positions where payloads can be inserted. These positions are highlighted in green and enclosed by section marks **(`§`)**.

> What symbol defines the start and the end of a payload position? §
> 

### **Task 4 - Payloads**

In the **Payloads** tab of Burp Suite Intruder, we can create, assign, and configure payloads for our attack. 

![image.png](Jr%20Pen%206%203%20Burp%20Suite%20Intruder/image%202.png)

Payload Sets: This section allows us to choose the position for which we want to configure a payload set and select the type of payload we want to use.

Payload settings: This section provides options specific to the selected payload type for the current payload set.

Payload Processing: In this section, we can define rules to be applied to each payload in the set before it is sent to the target.

Payload Encoding: The section allows us to customize the encoding options for our payloads.

> Which Payload processing rule could we use to add characters at the end of each payload in the set? Add suffix
> 

### **Task 5 - Introduction to Attack Types**

The Positions tab of Burp Suite Intruder has a dropdown menu for selecting the attack type. Intruder offers four attack types, each serving a specific purpose. 

**Sniper:** The Sniper attack type is the default and most commonly used option. It cycles through the payloads, inserting one payload at a time into each position defined in the request. Sniper attacks iterate through all the payloads in a linear fashion, allowing for precise and focused testing.
**Battering ram:** The Battering ram attack type differs from Sniper in that it sends all payloads simultaneously, each payload inserted into its respective position. This attack type is useful when testing for race conditions or when payloads need to be sent concurrently.
**Pitchfork:** The Pitchfork attack type enables the simultaneous testing of multiple positions with different payloads. It allows the tester to define multiple payload sets, each associated with a specific position in the request. Pitchfork attacks are effective when there are distinct parameters that need separate testing.
**Cluster bomb:** The Cluster bomb attack type combines the Sniper and Pitchfork approaches. It performs a Sniper-like attack on each position but simultaneously tests all payloads from each set. This attack type is useful when multiple positions have different payloads, and we want to test them all together.

> What attack type cycles through the payloads inserting one payload at a time into each position defined in the request? Sniper
> 

### **Task 6 - Sniper**

The **Sniper** attack type is the default and most commonly used attack type in Burp Suite Intruder. It is particularly effective for single-position attacks, such as password brute-force or fuzzing for API endpoints.  The Sniper attack type is beneficial when we want to perform tests with single-position attacks, utilizing different payloads for each position. It allows for precise testing and analysis of different payload variations.

> If you were using Sniper to fuzz three parameters in a request with a wordlist containing 100 words, how many requests would Burp Suite need to send to complete the attack? 300
> 

> How many sets of payloads will Sniper accept for conducting an attack? 1
> 

### **Task 7 - Battering Ram**

The Battering ram attack type in Burp Suite Intruder differs from Sniper in that it places the same payload in every position simultaneously, rather than substituting each payload into each position in turn.

As a hypothetical question: You need to perform a Battering ram Intruder attack on the example request above. If you have a wordlist with two words in it (admin and Guest) and the positions in the request template look like this: username=§pentester§&password=§Expl01ted§

> What would the body parameters of the first request that Burp Suite sends be? username=admin&password=admin
> 

### **Task 8 - Pitchfork**

The Pitchfork attack type in Burp Suite Intruder is similar to having multiple Sniper attacks running simultaneously. While Sniper uses one payload set to test all positions simultaneously, Pitchfork utilises one payload set per position (up to a maximum of 20) and iterates through them all simultaneously.

> What is the maximum number of payload sets we can load into Intruder in Pitchfork mode? 20
> 

### **Task 9 - Cluster Bomb**

The Cluster bomb attack type in Burp Suite Intruder allows us to choose multiple payload sets, one per position (up to a maximum of 20). Unlike Pitchfork, where all payload sets are tested simultaneously, Cluster bomb iterates through each payload set individually, ensuring that every possible combination of payloads is tested.

> We have three payload sets. The first set contains 100 lines, the second contains 2 lines, and the third contains 30 lines. How many requests will Intruder make using these payload sets in a Cluster bomb attack? 6000
> 

### **Task 10 - Practical Example**

To put our theoretical knowledge into practice, we will attempt to gain access to the support portal located at http://MACHINE_IP/support/login. This portal follows a typical login structure, and upon inspecting its source code, 

![image.png](Jr%20Pen%206%203%20Burp%20Suite%20Intruder/image%203.png)

![image.png](Jr%20Pen%206%203%20Burp%20Suite%20Intruder/image%204.png)

![image.png](Jr%20Pen%206%203%20Burp%20Suite%20Intruder/image%205.png)

![image.png](Jr%20Pen%206%203%20Burp%20Suite%20Intruder/image%206.png)

![image.png](Jr%20Pen%206%203%20Burp%20Suite%20Intruder/image%207.png)

**Tutorial:** 

Download and Prepare Wordlists, Navigate to http://MACHINE_IP/support/login in your browser. 

Send the captured request from the Proxy to Intruder by right-clicking and selecting "Send to Intruder"  In the "Positions" sub-tab, ensure that only the username and password parameters are selected. 

Set the Attack type to "Pitchfork.” Move to the "Payloads" sub-tab. You will find two payload sets available for the username and password fields.

In the first payload set (for usernames), go to "Payload Options," choose "Load," and select the usernames.txt list. Click the Start Attack button to begin the credential-stuffing attack. A warning about rate-limiting may appear; click OK to proceed. The attack will take a few minutes to complete in Burp Community.

Once the attack starts, a new window will display the results of the requests. However, as Burp sent 100 requests, we need to identify which one(s) were successful.

> What username and password combination indicates a successful login attempt? The answer format is "username:password".  m.rivera:letmein1
> 

### **Task 11 - Practical Challenge**

The numbering system indicates that the tickets are assigned integer identifiers rather than complex and hard-to-guess IDs. This information is significant because it suggests two possible scenarios:

Access Control: The endpoint may be properly configured to restrict access only to tickets assigned to our current user. In this case, we can only view tickets associated with our account.
IDOR Vulnerability: Alternatively, the endpoint may lack appropriate access controls, leading to a vulnerability known as Insecure Direct Object References (IDOR). If this is the case, we could potentially exploit the system and read all existing tickets, regardless of the assigned user.

![image.png](Jr%20Pen%206%203%20Burp%20Suite%20Intruder/image%208.png)

![image.png](Jr%20Pen%206%203%20Burp%20Suite%20Intruder/image%209.png)

> Which attack type is best suited for this task? Sniper
> 

> Either using the Response tab in the Attack Results window or by looking at each successful (i.e. 200 code) request manually in your browser, find the ticket that contains the flag. What is the flag?
> 

THM{MTMxNTg5NTUzMWM0OWRlYzUzMDVjMzJl}

### **Task 12 - Extra Mile Challenge**

Begin by capturing a request to http://MACHINE_IP/admin/login/ and reviewing the response.

![image.png](Jr%20Pen%206%203%20Burp%20Suite%20Intruder/image%2010.png)

**Tutorial:**
1. Navigate to http://MACHINE_IP/admin/login/. Activate Intercept in the Proxy module and attempt to log in. Capture the request and send it to Intruder.

2. Configure the positions the same way as we did for brute-forcing the support login:Set the attack type to "Pitchfork". Clear all predefined positions and select only the username and password form fields. Our macro will handle the other two positions.

3.Now switch over to the Payloads tab and load in the same username and password wordlists we used for the support login attack. Up until this point, we have configured Intruder in almost the same way as our previous credential stuffing attack; this is where things start to get more complicated.

4. With the username and password parameters handled, we now need to find a way to grab the ever-changing loginToken and session cookie. Unfortunately, "recursive grep" won't work here due to the redirect response, so we can't do this entirely within Intruder – we will need to build a macro. Macros allow us to perform the same set of actions repeatedly. In this case, we simply want to send a GET request to /admin/login/.  Fortunately, setting this up is a straightforward process.

Switch over to the main "Settings" tab at the top-right of Burp.
Click on the "Sessions" category.
Scroll down to the bottom of the category to the "Macros" section and click the Add button.
The menu that appears will show us our request history. If there isn't a GET request to http://MACHINE_IP/admin/login/ in the list already, navigate to this location in your browser, and you should see a suitable request appear in the list.
With the request selected, click OK.
Finally, give the macro a suitable name, then click OK again to finish the process.

5.Now that we have a macro defined, we need to set Session Handling rules that define how the macro should be used. Still in the "Sessions" category of the main settings, scroll up to the "Session Handling Rules" section and choose to Add a new rule. 

6. Now we need to switch back over to the Details tab and look at the "Rule Actions" section. Click the Add button – this will cause a dropdown menu to appear with a list of actions we can add.
Select "Run a Macro" from this list. In the new window that appears, select the macro we created earlier.

As it stands, this macro will now overwrite all of the parameters in our Intruder requests before we send them; this is great, as it means that we will get the loginTokens and session cookies added straight into our requests. That said, we should restrict which parameters and cookies are being updated before we start our attack:

Select "Update only the following parameters and headers", then click the Edit button next to the input box below the radio button.
In the "Enter a new item" text field, type "loginToken". Press Add, then Close.
Select "Update only the following cookies", then click the relevant Edit button.
Enter "session" in the "Enter a new item" text field. Press Add, then Close.
Finally, press OK to confirm our action.

7. You should now have a macro defined that will substitute in the CSRF token and session cookie. All that's left to do is switch back to Intruder and start the attack!

Note: You should be getting 302 status code responses for every request in this attack. If you see 403 errors, then your macro is not working properly.

8. As with the support login credential stuffing attack we carried out, the response codes here are all the same (302 Redirects). Once again, order your responses by length to find the valid credentials. Your results won't be quite as clear-cut as last time – you will see quite a few different response lengths: however, the response that indicates a successful login should still stand out as being significantly shorter.

9. Use the credentials you just found to log in (you may need to refresh the login page before entering the credentials).

### **Task 13 - Conclusion**

By now, you should have a solid understanding of how to effectively utilize Intruder to automate requests and perform various types of attacks.
