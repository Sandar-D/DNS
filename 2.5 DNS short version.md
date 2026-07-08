DNS, or a domain name system, translates human readable domain names into machine understandable IP addresses. 

**DNS resolution:**
When a DNS query is initiated, resolver on the local machine asked by the program to check the system caches first to see if the domain name and its corresponding IP address are already stored there. It will visit caches in the following order:
   - Browser cache
   - OS cache
   - Hosts file
   - Router cache
   - ISP cache
If the DNS zone files aren't cached, [resolver](https://www.nslookup.io/learning/what-is-a-dns-resolver/) will then convert the name resolution requests from applications like web browsers into DNS request messages. 

Local resolvers are aware only of your recursive resolvers which are configured in the /etc/resolv.conf file and the local resolver sends the DNS request messages to them. It'll ask if they know where to find the domain `touchsupport.com`, and at that point they'll take the request and start a recursive query from the first location they are aware of - root nameservers. 

**Root Nameservers**: Root nameservers are the initial point of contact for recursive resolvers in their search for DNS records. When a recursive resolver sends a query that includes a domain name, the root nameserver responds by directing the resolver to the appropriate TLD nameserver based on the domain's extension. These root nameservers are distributed across multiple locations worldwide.

*TLD (Top Level Domain) Nameservers**: TLD nameservers store information for domain names that share a common domain extension. For example, a .com TLD nameserver contains information for all websites with a ".com" extension. TLD nameservers can be categorized into two main groups:
  - Generic top-level domains (gTLDs): These are domain extensions that are not country-specific, such as .com, .org, .net, .edu, and .gov.
  - Country code top-level domains (ccTLDs): These include domain extensions specific to a country or state, such as .rs for Serbia.

After receiving a response from a root nameserver, the recursive resolver sends a query to the corresponding TLD nameserver for the requested domain. The TLD nameserver does not have access to the A record of the domain we need but it is aware of which authority manages the DNS zone for that domain. TLD-nameserver then points the recursive resolver toward the domain registrar that holds the information for authoritative nameservers where DNS zone is being hosted. 

**Domain registrar**: Once DNS resolver receives a response from TLD nameservers, it'll go to the registrar for the information regarding which authoritative nameservers hold the DNS zone file for a particular domain.

**Authoritative Nameservers**: Once the recursive resolver receives a response from the TLD nameserver, it is directed to an authoritative nameserver. The authoritative nameserver holds domain-specific information and can provide the recursive resolver with the IP address of the server associated with the domain (found in the DNS A record). If the authoritative nameserver contains an alias domain (CNAME record) for the requested domain, it provides the recursive resolver with the alias domain. In such cases, the recursive resolver would need to perform a new DNS lookup to obtain the necessary records from the authoritative nameserver.****

**Summary:** If the information isn't cached, a stub resolver queries a recursive resolver, which then sends queries to other nameservers in pursuit of an answer for the resolver. It will visit root nameserver ->TLD nameserver -> Domain Registrar -> Authoritative nameservers.  After the recursive resolver queries the authoritative nameservers, it returns an IP address as an answer for the client which requested the information.

