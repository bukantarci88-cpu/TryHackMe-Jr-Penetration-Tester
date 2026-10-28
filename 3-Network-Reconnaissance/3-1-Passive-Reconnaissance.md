# Jr.Pen 3.1. Passive Reconnaissance

#### **Passive Reconnaissance**

Passive reconnaissance mainly gathers information based on publicly available information. Learning Objectives:

- Use `whois` to query domain registration details.
- Use `dig` (and `nslookup` for compatibility) to query DNS records.
- Understand why querying public WHOIS and DNS servers is considered passive.
- Discover subdomains using DNSDumpster and Certificate Transparency logs.
- Gather intelligence on exposed services using Shodan.io.

#### **Step 1: Whois**

Whois is used to search for an object in an RFC 3912 database. It helps us to gather information such as contact information, registrar, registration date, registration updates and registration expiration dates, and contact emails, among others.

<img width="722" height="445" alt="image 1" src="https://github.com/user-attachments/assets/520df67d-3a17-427a-b7cd-cdfb91902ce3" />
<img width="731" height="368" alt="image" src="https://github.com/user-attachments/assets/0dc0ef5e-5917-4e66-a770-5970d23fd45b" />


Using Whois we were able to answer the following question asked on the challenge:

**Question 1**: When was TryHackMe.com registered? Answer: 20180705

**Question 2**: What is the registrar of TryHackMe.com? Answer: Namecheap.com

**Question 3**: Which company is TryHackMe.com using for name servers? Answer: Cloudflare.com

#### **Step 2: Nslookup And dig**

Nslookup (Name Server LookUp) is used to query Domain Name System (DNS) servers to map a domain name to an IP as well as other DNS records. We can use it non-interactively and pass arguments. We can use the -type flag, to specify the query type.

| **Query type** | **Result** |
| --- | --- |
| A | IPv4 address(es) for the domain |
| AAAA | IPv6 address(es) for the domain |
| CNAME | Canonical Name: an alias that points one domain name to another |
| MX | Mail Servers: the servers responsible for handling email for the domain |
| SOA | Start of Authority: the primary name server, admin email, and zone serial number |
| TXT | Text Records: arbitrary text, commonly used for SPF, DKIM, DMARC, and domain verification |

```
nslookup -type=A tryhackme.com 1.1.1.1
```

<img width="660" height="295" alt="image 2" src="https://github.com/user-attachments/assets/cf3ae6ac-c3e3-4407-adf2-b0b138ca15e8" />


nslookup (Name Server Lookup) is the older of the two tools.

1. nslookup DOMAIN_NAME performs a simple lookup using your default resolver.
2. nslookup -type=TYPE DOMAIN_NAME [SERVER] specifies a record type and an optional DNS server.

**Privacy tip**: Use public resolvers like `1.1.1.1` (which supports DNS over HTTPS and DNS over TLS) to avoid your ISP logging your queries.

the 1.1.1.1 part simply tells nslookup: “Use this specific DNS server instead of the system default.”

**Question 1:** Check the TXT records of thmlabs.com. What is the flag there?

> *Answer: THM{a5b83929888ed36acb0272971e438d78}*
> 

We have solved this question just by repeating similar queries as we saw above for the domain names “thmlabs.com.” and used  -type=txt and got the flag successfully.

```
nslookup -type=txt thmlabs.com
```

![nslookup on thmlabs.com](https://media.geeksforgeeks.org/wp-content/uploads/20220417175801/Screenshot20220417175323.png)

If we want more advanced DNS queries and additional functionality, we can use dig(“Domain Information Groper”).dig will return more information, such as the TTL (Time To Live) by default.

![dig tool output](https://media.geeksforgeeks.org/wp-content/uploads/20220417180004/Screenshot20220417175449.jpg)

uses the dig DNS tool to query the MX (Mail Exchange) records for [tryhackme.com](http://tryhackme.com/).

dig → DNS lookup utility (“Domain Information Groper”)
@1.1.1.1 → use Cloudflare DNS server
[tryhackme.com](http://tryhackme.com/) → domain to query
MX → request mail server records

MX records tell the internet: “Which mail servers handle email for this domain?”

#### **Step 3: DNSDumpster**

A lot of useful information can be gathered from the tools above but they are not designed for the enumeration of subdomains. Subdomains can have useful information and servers that can be vulnerable. DNSDumpster.com can be used to discover subdomains, and their IP addresses and attempt to geolocate them.

![DNSDumpster enumeration](https://media.geeksforgeeks.org/wp-content/uploads/20220417180247/Screenshot20220417175718.jpg)

![DNSDumpster results](https://media.geeksforgeeks.org/wp-content/uploads/20220417180246/Screenshot20220417175705.jpg)

Certificate Transparency (CT) Logs:  **Certificate Transparency** is a public logging framework (mandatory since approximately 2015) that records every SSL/TLS certificate issued by participating Certificate Authorities. Each certificate contains a **Subject Alternative Name (SAN)** field listing the domains and subdomains it covers. By searching these logs, you can discover subdomains without sending any traffic to the target.

To use crt.sh, visit `https://crt.sh` and search for `%.tryhackme.com`. The `%` wildcard matches any subdomain. The results will list every certificate issued for subdomains of `tryhackme.com`, often revealing 10 to 100 times more subdomains than DNSDumpster alone.

From the above graphical view, we found a domain called remote.tryhackme.com which help me to answer the following question:

**Question 1**: Lookup tryhackme.com on DNSDumpster. What is one interesting subdomain that we would discover in addition to www and blog? Answer: remote

#### **Step 4: Shodan**

Shodan.io is used to learn more about our target network. Shodan is like a search engine for devices online, and just because we are not directly connecting to the devices, it will give us a lot of information during the passive research. If we search for tryhackme.com on shodan we will get the following result.

To begin, navigate to `https://www.shodan.io`. No account is required for basic searches. Enter a domain name (e.g., `tryhackme.com`) or an IP address from your earlier DNS lookups (e.g., `104.26.10.229`) into the search bar.

The results page displays a list of matching hosts. Selecting a host opens a detailed view containing the following information:

- **IP address** and **ASN** (Autonomous System Number): identifies the network block.
- **Hosting provider/organisation** (e.g., Cloudflare, AWS): reveals the infrastructure behind the domain.
- **Geographic location** (country, city): approximate physical location of the server.
- **Open ports and services**: with version strings and banners (e.g., HTTP server type and version).
- **Tags**: such as `cdn` or `vuln` if a known vulnerability matches the detected service version.

![Shodan search](https://media.geeksforgeeks.org/wp-content/uploads/20220417180549/Screenshot20220417180051.png)

**Question 1**: According to Shodan.io, what is the 2nd country in the world in terms of the number of publicly accessible Apache servers? Answer: Germany

![Question 1 Answer](https://media.geeksforgeeks.org/wp-content/uploads/20220417180732/Screenshot20220417180254.png)

**Question 2**: Based on Shodan.io, what is the 3rd most common port used for Apache? Answer: 8080

![common port used for Apache](https://media.geeksforgeeks.org/wp-content/uploads/20220417181020/Screenshot20220417180543.png)

**Question 3**: Based on Shodan.io, what is the 3rd most common port used for Nginx? Answer: 8888
