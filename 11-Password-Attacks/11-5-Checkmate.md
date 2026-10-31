# Jr.Pen 11.5. Checkmate

**Challenge:** Marco Bianchi, a systems administrator, recently deployed several internal services, including a firewall console, employee portal, social platform, and SSH access to critical infrastructure. Due to tight deadlines and operational pressure, Marco reused weak, predictable, and pattern-based passwords across multiple systems. Your objective is to conduct a password security assessment to identify weaknesses in Marco’s authentication practices.

Before starting the room, I clicked the link provided to me and added firewall.thm to the /etc/hosts file to use the system's static hostname lookup table.

• **`/etc/hosts`**: A system file that maps IP addresses to hostnames (domain names) locally.   

<img width="662" height="251" alt="image" src="https://github.com/user-attachments/assets/e5fb0ba1-26d3-4485-92fa-7cc80608aa99" />


**Answer the questions below**

> What is the password for Level 1?
> 

12345

<img width="369" height="348" alt="image 1" src="https://github.com/user-attachments/assets/161af169-d89f-44f1-af2b-ed1686ffa619" />


As room says, I tried all the possibilities at random. (like admin - admin , admin - password). Once we log in with the default password 12345, the site forwards us to this page.

> What is the password for Level 2?
> 

excellence

<img width="822" height="320" alt="image 2" src="https://github.com/user-attachments/assets/d389eaef-0885-46f8-98a6-7194a5fc2cec" />


 After adding jobs.thm to /etc/hosts, I accessed the page and attempted the password listed on it.

<img width="923" height="326" alt="image 3" src="https://github.com/user-attachments/assets/a3b9e5d6-0a1c-4a0d-839f-0854fe95468c" />


We need to build a custom wordlist from the website's content and use it to brute-force the login page, using tools such as CeWL, curl, or wget.

run cewl command 

<img width="816" height="318" alt="image 4" src="https://github.com/user-attachments/assets/3b4bb410-7889-4c51-a975-96ee419e00e7" />


```bash
root@ip-10-112-103-128:~# cewl -d 2 -m 7 --lowercase --with-numbers -w cewl_words.txt [http://jobs.thm:5002](http://jobs.thm:5002/)
```

cewl: A tool that crawls websites to build custom wordlists for password testing. `-d 2`: Sets the **depth** of the web crawl to `2`. CeWL will start at the target URL,`-m 7`: Sets the **minimum word length** to `7` characters. 

> What is the password for Level 3?
> 

Bianchi2495

<img width="890" height="394" alt="image 5" src="https://github.com/user-attachments/assets/7752c66e-c3f4-419d-9797-ec88f920db01" />


After adding social.thm to /etc/hosts, I accessed the page and cloned the Mebus GitHub repository.

We'll use CUPP to build a targeted wordlist based on Marco's personal details.

<img width="841" height="239" alt="image 6" src="https://github.com/user-attachments/assets/16346871-2132-4dc5-8f96-e628eaeabf3a" />


```bash
root@ip-10-112-103-128:~# git clone [https://github.com/Mebus/cupp.git](https://github.com/Mebus/cupp.git)
```

This command clones the official GitHub repository for **CUPP** (Common User Passwords Profiler), a tool used to generate custom password lists based on target profiling data.

<img width="623" height="453" alt="image 7" src="https://github.com/user-attachments/assets/d72d9f68-3979-4be8-8b75-82bc199c7ff1" />


After generating the personalized wordlist, we will use it to brute-force the login page of the social platform using **Hydra**.

run hydra

```bash
root@ip-10-112-103-128:~/cupp# hydra -l marco -P marco.txt -f -V -t 4 social.thm http-post-form '/login:username=^USER^&password=^PASS^:F=Invalid' -s 5003
```

This command uses **Hydra**, a multi-threaded network logon cracker, to attempt a dictionary-based attack against a web-based login form running on host `social.thm` at port `5003`.

<img width="740" height="119" alt="image 8" src="https://github.com/user-attachments/assets/cb6af844-91f7-44b0-b287-53b38e1641ae" />


- **`hydra`**: Invokes the Hydra application.
- **`l marco`**: Specifies a single static username (`marco`) to target.
- **`P marco.txt`**: Specifies the wordlist file containing candidate passwords to test against the targeted account.
- **`f`**: Tells Hydra to exit immediately after finding the first valid password pair.
- **`V`**: Enables verbose output, showing each username and password attempt in real time in the terminal.
- **`t 4`**: Limits Hydra to `4` parallel tasks (threads) to avoid overloading the target web server or triggering strict rate limiting.
- **`social.thm`**: The domain name or IP address of the target host.
- **`http-post-form`**: Specifies the module used to handle HTTP POST request forms.
- **`'/login:username=^USER^&password=^PASS^:F=Invalid'`**: Defines the target URL path, HTTP body parameters, and success/failure conditions:
    - `/login`: The URL path where the login form processes requests.
    - `username=^USER^&password=^PASS^`: The POST body syntax, where `^USER^` is replaced by `marco` and `^PASS^` is replaced by entries from `marco.txt`.
    - `F=Invalid`: The failure condition. Hydra checks the HTTP response for the string `"Invalid"`. If that string is present, Hydra assumes the login failed; if it is missing, Hydra marks the attempt as successful.
- **`s 5003`**: Sets the target port to `5003` instead of the default HTTP port (`80`).

> What is the password for Level 4?
> 

family

<img width="841" height="405" alt="image 9" src="https://github.com/user-attachments/assets/61f0776d-9472-40c5-a57d-1cde53528679" />


After logged in the page , clicked the picture then save as 

<img width="896" height="219" alt="image 10" src="https://github.com/user-attachments/assets/9e046fa1-3bc3-4aa4-9ef3-d142eaa091c2" />


create vim hash.txt file 

```bash
root@ip-10-112-103-128:~/cupp# hashcat -m 1400 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```

<img width="808" height="137" alt="image 11" src="https://github.com/user-attachments/assets/c5365489-ffd4-4a25-b7a3-4aab0d3fe295" />


- **`m 1400`**: Specifies the **hash type** algorithm. Mode `1400` tells Hashcat to process the target hashes as **SHA256**.
- **`a 0`**: Sets the **attack mode**. Mode `0` corresponds to a **Straight / Dictionary attack**, where Hashcat tests words directly from a provided wordlist line by line.
- **`hash.txt`**: The file containing the raw hash(es) to be cracked.
- **`/usr/share/wordlists/rockyou.txt`**: The path to the wordlist file containing candidate passwords.

> What is the password for Level 5?
> 

Security2024!

<img width="819" height="365" alt="image 12" src="https://github.com/user-attachments/assets/3c728bb7-cdbd-46e8-8887-c88237d6d1cb" />

<img width="523" height="140" alt="image 13" src="https://github.com/user-attachments/assets/f2d771f0-d3bb-4e73-bd0c-360309550700" />


We'll use Crunch to generate a wordlist that matches the specified password format.

```bash
crunch 13 13 -t Security20%% ! -o security_wordlist.txt
```

- **`crunch`**: Invokes the Crunch utility.
- **`13 13`**: Sets both the **minimum length** and **maximum length** of the generated words to **13 characters**.
- **`t Security20%% !`**: Specifies a fixed pattern template (`t`) for generating words. In Crunch syntax, specific characters serve as placeholders for dynamic variations:
    - **Literal characters:** `Security20` remains static at the beginning of every word.
    - **`%`**: Represents a **numeric character** (`0–9`). Two `%` symbols mean two varying numbers will be generated in those positions (e.g., `00` through `99`).
    - **Space and `!`**: Trailing static characters.
- **`o security_wordlist.txt`**: Directs Crunch to save (**output**) the resulting wordlist to a text file named `security_wordlist.txt`.

Once the wordlist is ready, we'll use Hydra to brute-force the SSH service.

```bash
hydra -l marco -P security_wordlist.txt 10.48.149.185 ssh -V -t 4
```

head pass_marco.txt

<img width="345" height="212" alt="image 14" src="https://github.com/user-attachments/assets/2e97998a-9f85-421a-b5d5-a5155eace7f2" />
