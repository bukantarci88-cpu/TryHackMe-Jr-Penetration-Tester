# Jr. Pen 6.2. Burp Suite: Repeater

### **Task 1 - Introduction**

Explore the advanced capabilities of the Burp Suite framework by focusing on the Burp Suite Repeater module.

### **Task 2 - What is Repeater?**

Burp Suite Repeater enables us to modify and resend intercepted requests to a target of our choosing. It allows us to take requests captured in the Burp Proxy and manipulate them, sending them repeatedly as needed. 

**Request List:** Located at the top left of the tab, it displays the list of Repeater requests. Multiple requests can be managed simultaneously, and each new request sent to Repeater will appear here.

**Request Controls:** Positioned directly beneath the request list, these controls allow us to send a request, cancel a hanging request, and navigate through the request history.

**Request and Response View:** Occupying the majority of the interface, this section displays the Request and Response views. We can edit the request in the Request view and then forward it, while the corresponding response will be shown in the Response view.

**Layout Options:** Located at the top-right of the Request/Response view, these options enable us to customize the layout of the Request and Response views. The default setting is a side-by-side (horizontal) layout, but we can also choose a vertical layout or combine them in separate tabs.

**Inspector:** Positioned on the right-hand side, the Inspector allows us to analyze and modify requests in a more intuitive manner than using the raw editor. We will explore this feature in a later task.

**Target:** Situated above the Inspector, the Target field specifies the IP address or domain to which the requests are sent. When requests are sent to Repeater from other Burp Suite components, this field is automatically populated.

### **Task 3 - Basic Usage**

Once a request has been captured in the Proxy module, we can send it to Repeater by either right-clicking on the request and selecting Send to Repeater, or by utilizing the keyboard shortcut Ctrl + R. Shifting our focus back to Repeater.

> Which view will populate when sending a request from the Proxy module to Repeater? Request
> 

### **Task 4 - Message Analysis Toolbar**

Repeater provides us with various request and response presentation options, ranging from hexadecimal output to a fully rendered page. To explore these options, we can refer to the section located above the response box, where the following four view buttons are available:

![Four buttons above the response box](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/635ad62b0204b104bbd58489716eccde.png)

We are presented with the following display choices:

1. **Pretty**: This is the default option, which takes the raw response and applies slight formatting enhancements to improve readability.
2. **Raw**: This option displays the unmodified response directly received from the server without any additional formatting.
3. **Hex**: By selecting this view, we can examine the response in a byte-level representation, which is particularly useful when dealing with binary files.
4. **Render**: The render option allows us to visualize the page as it would appear in a web browser. While not commonly utilised in Repeater, as our focus is usually on the source code, it still offers a valuable feature. For most scenarios, the **Pretty** option is generally sufficient. However, it is beneficial to be acquainted with the usage of the other three options.

> Which option allows us to visualize the page as it would appear in a web browser? render
> 

### **Task 5 - Inspector**

Inspector is a supplementary feature to the Request and Response views in the Repeater module. It is also used to obtain a visually organized breakdown of requests and responses, as well as for experimenting to see how changes made using the higher-level Inspector affect the equivalent raw versions. Inspector can be utilized both in the Proxy and Repeater module. In both instances, it is situated on the far-right side of the window, presenting a list of components within the request and response:

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/6c95db764fe7491d938485256fd1dd87.png)

Among these components, the sections pertaining to the request can typically be modified, enabling the addition, editing, and removal of items. For instance, in the **Request Attributes** section, we can alter elements related to the location, method, and protocol of the request. This includes modifying the desired resource to retrieve, changing the HTTP method from GET to another variant, or switching the protocol from HTTP/1 to HTTP/2:

![Switching the protocol from HTTP/1 to HTTP/2](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/4f3a0f3714ceebbd6016e077369b48e8.png)

Other sections available for viewing and/or editing include:

1. **Request Query Parameters:** These refer to data sent to the server via the URL. For example, in a GET request like `https://admin.tryhackme.com/?redirect=false`, the query parameter **redirect** has a value of "false".
2. **Request Body Parameters:** Similar to query parameters, but specific to POST requests. Any data sent as part of a POST request will be displayed in this section, allowing us to modify the parameters before resending.
3. **Request Cookies:** This section contains a modifiable list of cookies sent with each request.
4. **Request Headers:** It enables us to view, access, and modify (including adding or removing) any headers sent with our requests. Editing these headers can be valuable when examining how a web server responds to unexpected headers.
5. **Response Headers:** This section displays the headers returned by the server in response to our request. It cannot be modified, as we have no control over the headers returned by the server. Note that this section becomes visible only after sending a request and receiving a response.

> Which section in Inspector is specific to POST requests? Body Parameters
> 

### **Task 6 - Practical Example**

Repeater is particularly well-suited for tasks requiring repetitive sending of similar requests, typically with minor modifications. This is particularly useful for activities such as manual testing for SQL Injection vulnerabilities, attempting to bypass web application firewall filters, or adjusting parameters in a form submission. 

### **Task 7 - Challenge**

In the previous task, we demonstrated the usage of Repeater by adding a header and sending a request. This served as an illustrative example for utilizing Repeater. Now, it's time for a straightforward challenge!

### **Task 8 - Extra-mile Challenge**

This task contains an extra-mile challenge, which means that it is a slightly harder, real-world application for Burp Repeater. If you feel comfortable performing a manual SQL Injection by yourself. Adding a single apostrophe (') is usually enough to cause the server to error when a simple SQLi is present.

### **Task 9 - Conclusion**

By now, you should have a solid understanding of effectively utilising Repeater to edit, manipulate, and resend requests. Additionally, you should have gained insight into the numerous practical applications of this tool.
