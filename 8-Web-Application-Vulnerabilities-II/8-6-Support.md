# Jr. Pen 8.6. Support

Pentest the Support Ops platform to exploit vulnerabilities and achieve RCE.

A new internal Support Operations Platform has been deployed to assist IT and helpdesk teams. The application handles user management, internal APIs, and system-level operations. However, security was not the primary focus during development. Several features rely on user-controlled input and weak trust boundaries.

RCE - Remote Code Execution (RCE) is a vulnerability that allows an attacker to run arbitrary code on a remote system. If exploited successfully, it often leads to full system compromise. Can you pentest the platform and escalate your access to achieve RCE on the server?

Started with **RustScan** to discover open ports.

```
root@ip-10-112-87-159:~# rusrscan -a 10.112.168.74
```


<img width="837" height="478" alt="image" src="https://github.com/user-attachments/assets/b26ea8dc-a23a-4212-88fc-4871ac289f1e" />


Then used **Gobuster** to find directories. 

```
root@ip-10-112-87-159:~# gobuster dir -u [http://10.112.168.74](http://10.112.168.74/) -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,bak.zip.js
```


<img width="826" height="592" alt="image 1" src="https://github.com/user-attachments/assets/1e5910d0-e4b5-4d7e-9797-4064f4af0cd9" />


 After entering the **Ragel** password, I proceeded to inspect the application.


<img width="826" height="592" alt="image 2" src="https://github.com/user-attachments/assets/22f126db-4199-499e-a3b4-8af1cc7487cb" />


**Answer the questions below**

> What is the flag value after logging in as admin?
> 

THM{I_AM_ADMIN999}


<img width="922" height="573" alt="image 3" src="https://github.com/user-attachments/assets/03525110-87b6-4c07-b8aa-5dd7f12b5348" />

<img width="953" height="860" alt="image 4" src="https://github.com/user-attachments/assets/0f63fea0-d75b-4f90-9c4b-6eaa94cf657a" />


> What is the content of the file /home/ubuntu/user.txt?
> 

THM{GOT_THE_FLAG001}
