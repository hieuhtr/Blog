# How DNS works

## Basic concept and information
- DNS stands for Domain Name System
- Its purpose is to resolve and translate human-readable website name to IPv4 or IPv6 address
- It's basically a large Database which resides on various servers around the world, that contains the names and IP address of various host/domains.
- Without DNS, we would only be able to visit any website directly via IP address, no human-readable anymore
- DNS works on both the TCP and UDP Protocols, **port 53**

## How does DNS work?
Whenever you visit a domain such as [github.com](https://github.com), the browser's journey begins >>>

**Step 1**: Request information
  - TODO: Type `github.com` to address bar of Chrome, it's a new request for browser 

**Step 2**: DNS cache on Chrome
  - TODO: DNS query on chrome's cache, see `chrome://net-internals/#dns`
  - True: Return IP address of this domain for making a request
  - False: Didn't know it before --> call OS (step3)

**Step 3**: DNS cache on OS
  - TODO: OS receive request from chrome, then check its DNS cache, see `mDNSResponder` on MacOS
  - True: Return IP of [github.com](https://github.com) for browser
  - False: OS didn't know it before same as browser, OS call the Resolver (Recursive DNS servers)

**Step 4**: The Resolver on ISP (Internet Service Provider)
  - TODO: local OS sends a DNS Query to the Resolver by using UDP Protocol over Port 53, the Resolver will check its cache to find IP for [github.com](https://github.com)
  - True: Return IP address for OS
  - False: ISP didn't know it before same as our OS. ISP will ask the **Root server**

**Step 5**: The **ROOT** servers - see [https://www.iana.org/domains/root/servers](https://www.iana.org/domains/root/servers)
  - Fact: We have 13 root server for DNS around the world. They don’t know the answer, but they know where to find it. 
  - TODO: Root look at the first part of request, reading from right to left **.com** <-- 
  - Direct our request to **Top-Level Domain** (TLD) name servers for **.com**, it's Verisign TLD 
  - ISP will store TLD information, no need ask the root again.

**Step 6**: The **TLD nameservers**
  - TODO: The TLD nameservers review the next part of our request - **github**
  - Direct our query to the nameservers responsible for this specific domain
  - These **Authoritative nameservers** are responsible for knowing all the information about a specific domain, which are stored in DNS records

**Step 7**: The **Authoritative nameservers** 
  - TODO: The Resolver (ISP) retrieves the A record for github.com from the **authoritative nameservers** and stores the record in ISP's local cache
  - More keywords: `time-to-live` value, Domain Registrar, types of records

**Step 8**: Receive the answer
  - TODO: Resolver returns the A record back to OS
  - OS stores the record in its cache, reads the IP address then passes information to Chrome
  - Chrome stores the record in its cache
 
#### Finally, Chrome opens a connection to the webserver and receives the site.

> This entire process, from start to finish, takes only **milliseconds** to complete.

Reference: 
1. https://howdns.works
2. http://dyn.com/blog/dns-why-its-important-how-it-works/
3. https://www.verisign.com/en_US/website-presence/online/how-dns-works

--
***Hieu Huynh*** Apr 09, 2017.