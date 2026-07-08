### A RECORD
 **A record:** stands for an address record and resolves a domain name to an IPv4 address—a 32-bit numeric address. This is the only record that can hold IPs:

| domain.com  | record type |   value   |  TTL  |
|:-----------:|:-----------:|:---------:|:-----:|
|      @      |      A      | 192.0.2.1 | 14400 |

- `@` symbol indicates that this is a record for the root domain;
- `TTL` - time to live is the amount of time that any nameserver is allowed to cache the data. After the time to live expires, the nameserver must discard the cached data and get new data from the authoritative nameservers. 
- Domains can have multiple A records. They don't have a priority list, when a browser checks for A records it will pick one at random and if it doesn't work it will go to the next one. 

***
### MX record:
**MX (Mail eXchanger)** record specifies which mail servers are responsible for accepting incoming email messages for a particular domain. When someone sends an email to an address at a particular domain, the sender's email server will look up the MX record for that domain in DNS to determine which mail server to deliver the email to.

| domain.com  | record type |       value       |  TTL  | priority |
|:-----------:|:-----------:|:-----------------:|:-----:| -------- |
|      @      |     MX      | mail1.domain.com  | 45000 | 11       |
|      @      |     MX      | mail2.domain.com  | 45000 | 22       |

- `Priority numbers` indicate preference and the lower a priority value is, the higher the preference. The server will always try mail1 first because 10 is lower than 20. If the result of a message send is failure, the server will default to mail2. When both priority numbers are the same, that means that there is a load balancing between the mail servers.

***
### TXT RECORD (SPF, DKIM, DMARC)

**TXT record** contains important information about the domain. They are most commonly used to prevent spam email by authenticating message coming from a trusted source. 

| domain.com  | record type |   value    |  TTL  |
|:-----------:|:-----------:|:----------:|:-----:|
|      @      |     TXT     | string val | 32600 |

**SPF (sender policy framework)** maintains a list of IP addresses that are authorized to send email from a particular domain. By adding an SPF record to your Domain Name System (DNS), you can provide a public list of senders that are approved to send email from your domain. Inbound servers can cross-check that the email originated from a server with permission to send on your domains’ behalf by using DNS lookup.

  `v=spf1 a mx ip4:<IP_for_domain.com> ~all`
- Qualifiers: 
	- "+" - Pass: A mailer that matches is a valid sender.
	- "-" - Fail: A mailer that matches is NOT a valid sender.
	- "~" - Soft fail: A mailer that matches probably isn't a valid sender, so the message should be carefully scrutinized.
	- "?'" - Neutral: Has no effect
- Mechanisms:
	-  "all" - This mechanism always matches. It usually goes at the end of the SPF record.
	- "a" - Specifies the domain name of a mail server whos address or addresses are allowed to send email from the owner domain name.
	- "mx" - Specifies a domain name whose email exchanges are allowed to send email from the owner domain name.
	- "ip4" - Specifies the IP(v4) addresses of a mail server that is allowed to send email from the owner domain name. Can also specify a network in CIDR notation. (e.g., 192.168.0.0/24). 
	- "ip6" - Specifies the IPv6 address of a mail server that is allowed to send email from the owner domain name. 
	- "ptr" - Requires that a PTR record exist for the sending mail server's address. The PTR record must map the address to a domain name that ends in the domain name in the owner filed of the TXT record or the argument specified after the colon. For example, ptr:example.com requires that a sending mail server's address reversemap to a domain ending in example.com.
	- "exists" - Checks whether or not an A record is valid for a given domain. It works by looking at all A records on that domain and seeing if any of them match the criteria set out in your SPF record.
	- "include" - The “include” mechanism allows SPF to include other SPF records within a domain’s record. If a domain does not have an SPF record, but another domain does and that other domain has an IP address that matches the IP address of the sender, then the “include” mechanism will cause the domain with the matching IP address to be used for authorization purposes. This is designed to let administrators refer to SPF instructions configured by someone else.
- Modifier:
	- "redirect" - The redirect modifier is a string that replaces the entire domain name in the SPF record. The purpose of this modifier is to redirect all mail sent to the domain to another server. This may be useful for domains with multiple MX records or for domains that have been re-assigned to another company but are still using the same email addresses.
	- "exp" - value that specifies an explanation for why a message was rejected. It is intended to help senders avoid certain kinds of issues, and can be used to inform them about the specific reason their message was not accepted by the receiving server.

**DKIM (DomainKeys Identified Mail)** uses a pair of asymmetric keys to link an email back to the domain. There are two main aspects of DKIM: a DKIM record in the DNS zone which holds the public key, and the DKIM header found in all emails that holds the private key. A receiving email server will check the DKIM DNS record for a sending domain, obtain the public key, and use the public key to verify the private key inside of a header of an email.
  
DKIM Record example:
```
NAME: [selector]._domainkey.domain.com
CONTENT: v=DKIM1; p=76E629F05F70  
					9EF665853333  
					EEC3F5ADE69A  
					2362BECE4065  
					8267AB2FC3CB  
					6CBE
```
- NAME: The `selector` is a specialized value issued by the email service provider used by the domain. It is included in the DKIM header to enable an email server to perform the required DKIM lookup in the DNS. The `domain` is the email domain name. `._domainkey.` is included in all DKIM record names.
- CONTENT: This is the part of the DKIM DNS record that lists the public key. In the example above, `v=DKIM1` indicates that this TXT record should be interpreted as DKIM, and the public key is everything after `p=`.

DKIM header example (Not required):

```
v=1; a=rsa-sha256; 
        d=domain.com; s=[selector];
        h=from:to:subject;
	    bh=uMixy0BsCqhbru4fqPZQdeZY5Pq865sNAnOAxNgUS0s=;
b=LiIvJeRyqMo0gngiCygwpiKphJjYezb5kXBKCNj8DqRVcCk7obK6OUg4o+EufEbtRYQfQhgIkx5m70IqA6dP+DBZUcsJyS9C+vm2xRK7qyHi2hUFpYS5pkeiNVoQk/Wk4wZG4tu/g+OA49mS7VX+64FXr79MPwOMRRmJ3lNwJU=
```
- `v=` shows which version of DKIM is in use.
- `a=` is the algorithm used to compute the digital signature, or `b`, as well as generate the hash of the email body, or `bh`. In this example, RSA-SHA-256 is in use (RSA using SHA-256 as the hash function for the digital signature, and SHA-256 for the body hash).
- `d=` is the domain name of the sender.
- `s=` is the selector that the receiving server should use to look up the DNS record.
- `h=` lists the header fields that are used to create the digital signature, or `b`. In this case, the from, to, and subject headers are used. If Bob sent an email to Alice using the example.com domain and the subject line was "Recipe for cheesecake," the content used here would be "bob@domain.com" + "alice@domain.com" + "Recipe for cheesecake". (This content would also be canonicalized — put into a standardized format.)
- `bh=` is the hash of the email body. A hash is the result of a specialized mathematical function called a hash function. This is included so that the receiving email server can compute the signature before the entire email body loads, since email bodies can be any length and loading it may take a long time in some cases.
- `b=` is the **digital signature**, generated from `h` and `bh` and signed with the private key.

The digital signature (`b=`) allows the receiving server to 1. authenticate the sending server and 2. ensure integrity — that the email has not been tampered with.

The receiving server does this by taking the same content that is listed in `h=` plus the body hash (`bh=`) and using the public key from the DKIM record to check if the digital signature is valid. If the correct private key was used and if the content (headers and body) has not been altered, the email passes the DKIM check.

**DMARC record (Domain-based Message Authentication Reporting and Conformance)** is build on top of SPF and DKIM records and tells a receiving email server what to do after checking a domain's SPF and DKIM records. An email either passes or fails SPF and DKIM and the DMARC policy determines if failure results in the email being marked as spam, getting blocked, or being delivered to its intended recipient. Email servers may still mark emails as spam if there is no DMARC record, but DMARC provides clearer instructions on when to do so.

```
v=DMARC1; p=quarantine; adkim=s; aspf=s;
```

- `v=DMARC1` indicates that this TXT record contains a DMARC policy and should be interpreted as such by email servers.
- `p=quarantine` indicates that email servers should "quarantine" emails that fail DKIM and SPF — considering them to be potentially spam. Other possible settings for this include `p=none`, which allows emails that fail to still go through, and `p=reject`, which instructs email servers to block emails that fail.
- `adkim=s` means that DKIM checks are "strict." This can also be set to "relaxed" by changing the `s` to an `r`, like `adkim=r`.
- `aspf=s` is the same as `adkim=s`, but for SPF.

***
### CNAME RECORD

**The CNAME (canonical name) record** is used to map a domain or subdomain to another domain. For example, if we want both example1.com and example2.com to point to the same website, we can use a CNAME record. It is useful when running multiple services from a single IP address, such as www.domain.com and ftp.domain.com.  A CNAME must always point to another domain and not directly to an IP address.

| domain.com | record type |     value      |  TTL  |
|:----------:|:-----------:|:--------------:|:-----:|
|     @      |    CNAME    | www.domain.com | 32500 |

***
### NS RECORD

**The NS record (nameserver)** provides the name of the authoritative nameserver within a domain. A nameserver is a DNS server that stores all DNS records for a domain, including A records, MX records, or CNAME records (DNS zone). 

| domain.com  | record type |      value      |  TTL  |
|:-----------:|:-----------:|:---------------:|:-----:|
|      @      |     NS      | ns1.domain.com  | 21600 |

DNS zone is an administrative space that holds all the DNS records for a specific domain. Think of it as a directory that holds all the files. That directory then has to be stored somewhere, in this case on an authoritative nameserver. NS records connect that DNS zone to the authoritative nameserver responsible for them. 

***
### GLUE RECORD

[GLUE records](https://en.wikipedia.org/wiki/Domain_Name_System) - are in essence A records for nameservers. They are stored in the domain registrar along with the nameservers where the DNS zone is hosted in order to avoid the circular dependency problem.

Example: If the authoritative nameserver for the domain.com is ns1.domain.com, a computer trying to resolve domain.com will need to resolve ns1.domain.com first to find the server hosting domain.com. However, as the ns1 subdomain is configured in domain.com, computer attempting to access it will then have to find the DNS zone for domain.com first.

This presents a circular dependency and to break break it, we use glue records which are defined in the domain registrar. Glue records are A records that provide IP addresses for ns1.domain.com. The resolver uses one or more of these IP addresses to query one of the domain's authoritative servers, which allows it to complete the DNS query.

***
### PTR RECORD

**The PTR (pointer) record** functions as a reverse of an A record. It resolves IP addresses to domain names and is commonly used to prevent email spam. PTR records store IP addresses with their segments reversed, and they append ".in-addr.arpa" to that. For example if a domain has an IP address of 192.0.2.1, the PTR record will store the domain's information under:
`1.2.0.192.in-addr.arpa.

DNS PTR records are used in [reverse DNS lookups](https://www.cloudflare.com/learning/dns/glossary/reverse-dns/). When a user attempts to reach a domain name in their browser, a DNS lookup occurs, matching the domain name to the IP address. A reverse DNS lookup is the opposite of this process: it is a query that starts with the IP address and looks up the domain name.

Common uses for reverse DNS include:
**Anti-spam:** Some email anti-spam filters use reverse DNS to check the domain names of email addresses and see if the associated IP addresses are likely to be used by legitimate email servers.
**Troubleshooting email delivery issues:** Because anti-spam filters perform these checks, email delivery problems can result from a misconfigured or missing PTR record. If a domain has no PTR record, or if the PTR record contains the wrong domain, email services may block all emails from that domain.
**Logging:** System logs typically record only IP addresses; a reverse DNS lookup can convert these into domain names for logs that are more human-readable.

***
### AAAA record (Quad A record) 

AAAA functions similarly to an A record but resolves domain names to IPv6 addresses - 128-bit alphanumeric addresses replacing older IPv4 addresses as we are running out of the possible permutations.

***
### SRV RECORD

**The SRV record (Service)** points to a server and a specific service by including a port number. Let say you want to run a video or/and audio connection on a specific port of your server. This is where the SRV record comes in. It allows you to specify how your domain name handles this particular service. The SRV record makes it easier for web applications, such as chat, audio/video streaming services, VoIP, and others to connect to the relevant server on the correct port number. 

- **Type:** SRV
- **TTL:** 1 Hour
- **Host:** _service._protocol e.g.: _sip._tcp
- **Priority**: (From 0 to 65535) The priority of the target host, lower value means more preferred.
- **Weight:** From 0 to 65535) A relative weight for records with the same priority, higher value means more preferred.
- **Port:** (From 0 to 65535) The TCP or UDP port on which the service is to be found.1
- **Points to:** The canonical hostname of the machine providing the service, ending in a dot.

***
### SOA RECORD

**The SOA record (start of authority)** stores administrative information about a DNS zone. A DNS zone is a section of a domain name space delegated to a specific administration. DNS zones allow for the division of a domain namespace into different sections.

|    name     |     example.com      |
|:-----------:|:--------------------:|
| record type |         SOA          |
|    MNAME    | ns.primaryserver.com |
|    RNAME    |  admin.domain.com    |
|   SERIAL    |      1111111111      |
|   REFRESH   |        86400         |
|    RETRY    |         7200         |
|   EXPIRE    |       4000000        |
|     TTL     |        11200         |

-  `MNAME` is the name of the primary nameserver for the zone. Secondary servers that maintain duplicates of the zone's DNS records receive updates to the zone from this primary server.
- `The 'RNAME' value` represents the administrator's email address, and in an SOA record admin.example.com is the equivalent of admin@domain.com
- `Serial Number` - A zone serial number is a version number for the SOA record. When the serial number changes in a zone file, this alerts secondary nameservers that they should update their copies of the zone file via a zone transfer.
- `REFRESH` - the number of seconds that secondary servers should wait before asking primary servers for the SOA record to see if it has been updated.
- `RETRY ` - the length of time a server should wait for asking an unresponsive primary nameserver for an update again. 
- `EXPIRE` - If a secondary server does not get a response from the primary server for this amount of time, it should stop responding to queries for the zone.
- `Zone transfer` - is the process of sending DNS record data from a [primary](https://www.cloudflare.com/learning/dns/glossary/primary-secondary-dns/) nameserver to a secondary nameserver. The SOA record is transferred first. The serial number tells the secondary server if its version needs to be updated. Zone transfers take place over the [TCP protocol](https://www.cloudflare.com/learning/ddos/glossary/tcp-ip/).

### CAA RECORD

**Certification Authority Authorization (CAA) record**  allow domain owners to declare which certificate authorities are allowed to issue a certificate for a domain. They also provide a means of indicating notification rules if someone requests a certificate from an unauthorized certificate authority. If no CAA record is present, any CA is allowed to issue a certificate for the domain. If a CAA record is present, only the CAs listed in the record(s) are allowed to issue certificates for that hostname.

CAA records can set policy for the entire domain, or for specific hostnames, and are inherited by subdomains. For example, a CAA record set on `example.com` also applies to `subdomain.example.com`.

***
