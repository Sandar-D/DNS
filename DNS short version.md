DNS, or a domain name system, translates human readable domain names into machine understandable IP addresses. 

**DNS resolution:**
When a DNS query is initiated, resolver on the local machine is asked by a program to check the system caches to see if the domain name and its corresponding IP address are already stored there. It will typically visit caches in the following order:
   - Browser cache
   - OS cache
   - Hosts file
   - Router cache
   - ISP cache

If the DNS zone files aren't cached, [resolver](https://www.nslookup.io/learning/what-is-a-dns-resolver/) will then convert the name resolution requests  from applications like web browsers into DNS request messages. 

Local resolvers are aware only of your recursive resolvers and it sends the DNS request messages to them. Example of recursive resolvers are Goodle (@8.8.8.8) and Cloudflare (@1.1.1.1).

It'll ask if they know where to find the domain `example.com`, and at that point they'll take the request and start a recursive query from the first location they are aware of - root nameservers. 

**Root Nameservers**: Root nameservers are the initial point of contact for the recursive resolvers in their search for DNS records. When a recursive resolver sends a query that includes a domain name, the root nameserver responds by directing the resolver to the appropriate TLD nameserver based on the domain's extension. 

The root nameserver does not know the IP address associated with the requested domain. Its role is simply to direct the recursive resolver to the appropriate Top-Level Domain (TLD) nameserver based on the domain's extension. For example, when resolving `example.com`, the root nameserver responds with a referral to the `.com` TLD nameservers rather than the domain's DNS records.

**TLD (Top Level Domain) Nameservers**: TLD nameservers store information for domain names that share a common domain extension. For example, a .com TLD nameserver contains information for all websites with a ".com" extension. TLD nameservers can be categorized into two main groups:
  - Generic top-level domains (gTLDs): These are domain extensions that are not country-specific, such as .com, .org, .net, .edu, and .gov.
  - Country code top-level domains (ccTLDs): These include domain extensions specific to a country or state, such as .rs for Serbia.

After receiving a response from a root nameserver, the recursive resolver sends a query to the appropriate TLD nameserver for the requested domain. The TLD nameserver does not store the domain's A record. Instead, it contains delegation information (NS records) that identifies the authoritative nameservers responsible for the domain's DNS zone. It responds by directing the recursive resolver to those authoritative nameservers, which host the domain's DNS records.

**Domain registrar**: A domain registrar is responsible for registering domain names and maintaining the domain's registration information. When a domain owner configures or changes the authoritative nameservers for a domain, the registrar updates the domain's delegation information, which is then published in the corresponding TLD zone. During normal DNS resolution, however, recursive resolvers do not contact the domain registrar.

**Authoritative Nameservers**: Once the recursive resolver receives a response from the TLD nameserver, it is directed to an authoritative nameserver. The authoritative nameserver holds domain-specific information and can provide the recursive resolver with the IP address of the server associated with the domain (found in the DNS A record). If the authoritative nameserver contains an alias domain (CNAME record) for the requested domain, it provides the recursive resolver with the alias domain. In such cases, the recursive resolver would need to perform a new DNS lookup to obtain the necessary records from the authoritative nameserver.

**Summary:** If the information isn't cached, a local resolver queries a recursive resolver, which then sends queries to other nameservers in pursuit of an answer for the resolver. It will visit root nameserver ->TLD nameserver -> Domain Registrar -> Authoritative nameservers.  After the recursive resolver queries the authoritative nameservers, it returns an IP address as an answer for the client which requested the information.

