# Jr. Pen. 6. 4. Burp Suite: Other Modules

### **Task 1: Introduction**

They facilitate operations with encoded text, enable comparison of data sets, allow the analysis of randomness within captured tokens, and help you store and annotate copies of HTTP messages that you may want to revisit later.

### **Task 2: Decoder**

The Decoder module of Burp Suite gives user data manipulation capabilities. As implied by its name, it not only decodes data intercepted during an attack but also provides the function to encode our own data, prepping it for transmission to the target. 

<img width="873" height="212" alt="image" src="https://github.com/user-attachments/assets/35328565-56d0-42f4-ab45-280b73e17fc2" />


> Question 1: Which feature attempts auto-decode of the input? Smart decode
> 

### **Task 3: Decoder (Encoding/Decoding)**

Plain: This refers to the raw text before any transformations are applied.

URL: URL encoding is utilized to ensure the safe transfer of data in the URL of a web request.

**HTML**: HTML Entities encoding replaces special characters with an ampersand (&), followed by either a hexadecimal number or a reference to the character being escaped, and ending with a semicolon (;)

Base64: Base64, a commonly used encoding method, converts any data into an ASCII-compatible format. 

ASCII Hex: This option transitions data between ASCII and hexadecimal representations. 

Gzip: Gzip compresses data, reducing file and page sizes before browser transmission.

Hex Format
While inputting data in ASCII format is beneficial, there are times when byte-by-byte input editing is necessary. 

Smart Decode
Lastly, we have the Smart decode option. This feature tries to auto-decode encoded text.

> Question 2: Base64 encode the phrase: Let’s Start Simple. What is the base64 encoded version of this text? Answer: TGV0J3MgU3RhcnQgU2ltcGxl
> 

> Question 3: URL Decode this data: %4e%65%78%74%3a%20%44%65%63%6f%64%69%6e%67. What is the plaintext returned? Answer: Next:Decoding
> 

> Question 4: Use Smart decode to decode this data: %34%37. What is the decoded text? Answer: 47
> 

> Question 5: Encode this phrase: Encoding Challenge. Start with base64 encoding. Take the output of this and convert it into ASCII Hex. Finally, encode the hex string into octal. What is the final string? Answer: 24034214a720270024142d541357471232250253552c1162d1206c
> 

### **Task 4: Decoder (Hashing)**

Hash In addition to its Encoding/Decoding functionality, Decoder also offers the ability to generate hashsums for our data. The MD5 algorithm is deprecated and should not be used for contemporary applications.

> Question 6: Using Decoder, what is the SHA-256 hashsum of the phrase: Let’s get Hashing!? Convert this into an ASCII Hex string for the answer to this question.
> 

Answer: 6b72350e719a8ef5af560830164b13596cb582757437e21d1879502072238abe

> Question 7: Generate an MD4 hashsum of the phrase: Insecure Algorithms. Encode this as base64 (not ASCII Hex) before submitting.
> 

Answer: TcV4QGZZN7y7lwYFRMMoeA==

### **Task 5: Comparer**

Comparer, as the name implies, lets us compare two pieces of data, either by ASCII words or by bytes.


<img width="874" height="612" alt="image 1" src="https://github.com/user-attachments/assets/e882f798-793f-46a4-84d9-c2e5c9e55ae2" />


Just like most Burp Suite modules, we can also load data into Comparer from other modules by right-clicking and choosing Send to Comparer. Once we’ve added at least 2 datasets to compare and press on either Words or Bytes, a pop-up window shows us the comparison:


<img width="862" height="424" alt="image 2" src="https://github.com/user-attachments/assets/6291d032-10b2-4829-92e0-bb8e82b214ef" />


This window also has three distinct sections:

1. The compared data occupies most of the window; it can be viewed in either text or hex format. The initial format depends on whether we chose to compare by words or bytes in the previous window, but this can be overridden by using the buttons above the comparison boxes.
2. The comparison key is at the bottom left, showing which colors represent modified, deleted, and added data between the two datasets.
3. The **Sync views** checkbox is at the bottom right of the window. When selected, it ensures that both sets of data will sync formats. In other words, if you change one of them into Hex view, the other will adjust to match.

The window title displays the total number of differences found.

### **Task 6: Comparer (Example)**

There are many situations where being able to quickly compare two (potentially very large) pieces of data can come in handy.


<img width="675" height="496" alt="image 3" src="https://github.com/user-attachments/assets/1e1af20f-a02e-4fa6-b549-725711b69bc6" />

<img width="860" height="705" alt="image 4" src="https://github.com/user-attachments/assets/fee5a25d-cdf5-45a0-9581-d139d2d239c0" />


**Practical Example**
Navigate to [http://MACHINE_IP/support/login](http://MACHINE_IP/support/login) Try to log in with an invalid username and password – capture the request in the Burp Proxy.

Send the request to Repeater with Ctrl + R (or Mac equivalent) or by right-clicking on the request in the Proxy module and choosing Send to Repeater.

Send the request, then right-click on the response and choose Send to Comparer.

In the Repeater tab, change the credentials to:

Username: support_admin
Password: w58ySK4W
Send the request again, then pass the new response to Comparer.

### **Task 7: Sequencer (Overview)**

Sequencer allows us to evaluate the entropy, or randomness, of “tokens”. Tokens are strings used to identify something and should ideally be generated in a cryptographically secure manner. These tokens could be session cookies or Cross-Site Request Forgery (CSRF) tokens used to protect form submissions.


<img width="878" height="581" alt="image 5" src="https://github.com/user-attachments/assets/bae4652c-4733-4121-a923-610a3428c316" />


Live Capture: This is the more common method and is the default sub-tab for Sequencer. Live capture lets us pass a request that will generate a token to Sequencer for analysis. 

Manual Load: This allows us to load a list of pre-generated token samples directly into Sequencer for analysis. 

> What does Sequencer allow us to evaluate? Entropy
> 

### **Task 8: Sequencer (Live Capture)**


<img width="544" height="442" alt="image 6" src="https://github.com/user-attachments/assets/bb8de77e-8e2e-4c91-8b26-f65cdf621d01" />


In the “Token Location Within Response” section, we can select between Cookie, Form field, and Custom location. In this situation, we can safely leave all other options at their default values. So, click on the Start live capture button.


<img width="783" height="579" alt="image 7" src="https://github.com/user-attachments/assets/f37c042a-8fe4-4e0f-ba61-a28a70cd2461" />


Once around 10,000 tokens are captured, click on **Pause** and then select the **Analyze now button.** It’s important to note that we could have also chosen to **Stop** the capture. However, by opting to pause, we keep the option to resume the capture later if the report doesn’t have enough samples to accurately calculate the token’s entropy.

> What is the overall quality of randomness estimated to be? excellent
> 

### **Task 9: Sequencer (Analysis)**

The generated entropy analysis report is split into four primary sections. The first of these is the **Summary** of the results. The summary gives us the following:


<img width="866" height="616" alt="image 8" src="https://github.com/user-attachments/assets/fad832fb-fe26-4ab7-9d7d-34fb86437cb2" />


**Overall result**: This gives a broad assessment of the security of the token generation mechanism.
**Effective entropy**: This measures the randomness of the tokens. 
**Reliability**: The significance level of 1% implies that there is 99% confidence in the accuracy of the results. 

**Sample**: This provides details about the token samples analyzed during the entropy testing process, including the number of tokens and their characteristics

### **Task 10: Organizer**

The Organizer module of Burp Suite is designed to help you store and annotate copies of HTTP requests that you may want to revisit later. This tool can be particularly useful for organizing your penetration testing workflow.  You can send HTTP requests to Burp Organizer from other Burp Modules such as Proxy or Repeater. You can do this by right-clicking the request and selecting Send to Organizer or using the default hotkey Ctrl + O. Each HTTP request that you send to Organizer is a read-only copy of the original request saved at the point you sent it to Organizer.


<img width="867" height="545" alt="image 9" src="https://github.com/user-attachments/assets/a2170abb-89b4-4208-a086-00dccc29466d" />


Requests are stored in a table, which contains columns such as the request index number, the time the request was made, workflow status, Burp tool that the request was sent from, HTTP method, server hostname, URL file path, URL query string, number of parameters in the request, HTTP status code of the response, length of the response in bytes, and any notes that you have made.


<img width="871" height="565" alt="image 10" src="https://github.com/user-attachments/assets/b9010eb1-28ca-4c13-aac5-8aaf3e057526" />


To view the request and response:

1. Click on any Organizer item.
2. The request and response are both read-only. You can search within the request or response, select the request, and then use the search bar below the request.

> Are saved requests read-only? (yea/nay) yea
> 

### **Task 11: Conclusion**

**Decoder** allows you to encode and decode data, making it easier to read and understand the information being transferred.

**Comparer** enables you to spot differences between two datasets, which can be pivotal in identifying vulnerabilities or anomalies.

**Sequencer** helps in performing entropy analysis on tokens, providing insights into the randomness of their generation and, consequently, their security level.

·**Organizer** enables you to store and annotate copies of HTTP requests that you may want to revisit later.
