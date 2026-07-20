### IP address:

IP address is a unique value representing a location of the machine on the network. IP can be divided into several sections:

**By version:**
- IPv4 - 32-bit numeric value consisting of four octets (eight bits) separated by dots. It has 2^32 combinations, or around 4.3 billion. 
- IPv6 - 128-bit alphanumeric (hexadecimal) value and has 2^128 combinations, which is around 340 trillion trillion trillion.

**By address portion:**
- Network address is the ID that is assigned to the network so every network will have an unique address. 
- Host address is the ID that is assigned to a host device within that network. 

**By assignment:**
- Dynamic IP address - whenever a device attempts to gain access to the network, it will send a request to a DHCP (Dynamic host configuration protocol) server. The server will respond to the device with an available IP address and monitor the use of the address. Upon completion of the Internet access, device will return the IP it used back to the DHCP server pool to be reassigned to another device as it seeks access to the network.  DHCP components: DHCP server, client, IP address pool, subnet, lease and DHCP relay. 
- Static IP address is the one that is manually created and does not change over time. In the beginning, any time you wanted to add a device to the network you would have to access network configuration page and manually configure its IP address, subnet mask, default gateway and the DNS server: 

By network type:
- Public IP address** is the one that is assigned to your router by the Internet Service Provider. This address will be registered on the Internet and it will provide you access to World Wide Web. They are also unique, as in there are no duplicate public IPs anywhere in the world. 
- Private IP address is used only on the internal networks. A router will assign each device on the private network a private IP address using a DHCP service. Whenever that device attempts to access the Internet, router will act as an intermediary and translate the private address into public IP address (and vice versa) using the NAT service (Network Address Translation). 

<div style="page-break-after: always;"></div>

### Private IP classes:
Until the early 1990s, IP addresses were allocated using the classful addressing system. The total length of the address was fixed, and the number of bits allocated to the network and host portions were also fixed.

| Classes | Ranges                      | Default Subnet Mask |
| ------- | --------------------------- | ------------------- |
| A       | 10.0.0.0-10.255.255.255     | 255.0.0.0           |
| B       | 172.16.0.0-172.31.255.255   | 255.255.0.0         |
| C       | 192.168.0.0-192.168.255.255 | 255.255.255.0       |

In a classful addressing system, each class supported a fixed number of devices:
- Class A supported 16,777,214 hosts
- Class B supported 65,534 hosts
- Class C supported 254 hosts

The classful arrangement was inefficient when allocating IP addresses and led to a waste of IP address spaces.

For example, an organization with 300 devices couldn’t have used a Class C IP address, which only permitted 254 devices. So, the organization would’ve been forced to apply for a Class B IP address, which provided 65,534 unique host addresses. However, only 300 devices would’ve been connected, which would’ve left 65,234 unused IP address spaces.

### Subnetting
To resolve the inflexible distribution of IP addresses provided by the class system, subnetting system was introduce. 

Subnet Mask is a number that reveals how many bits in the IP address are used for the network by masking the network portion.  As computers only understand binary numbers, it is necessary to translate the IP address and subnet mask into their binary forms:

| 128 | 64  | 32  | 16  | 8   | 4   | 2   |  1  | 
| --- | --- | --- | --- | --- | --- | --- | --- |

Adding the numbers above, we get a 0-255 IP range. By putting a number 1 under each of the numbers above, we can add them to get the number in an octet:

| 192      | 168      | 1        | 0        |
| -------- | -------- | -------- | -------- |
| 11000000 | 10101000 | 00000001 | 00000000 |

Subnet mask binary conversion works in a similar manner:

| 255      | 255      | 255      | 0        |
| -------- | -------- | -------- | -------- |
| 11111111 | 11111111 | 11111111 | 00000000 |

By crossing out all of the 1 from the subnet mask with the numbers in the IP address, we can determine which portion of the address belongs to the network and which is the host portion:

| 11111111 | 11111111 | 11111111 | 00000000 |
| -------- | -------- | -------- | -------- |
| 11000000 | 10101000 | 00000001 | 00000000 |

The reason why a network and host portion for the IP address exist is manageability. It breaks down large networks into smaller networks, also known as subnetting. 

Subnet mask:

| Classs | First octet address | Default subnet mask |
| ------ | ------------------- | ------------------- |
| A      | 1-126               | 255.0.0.0           |
| B      | 128-191             | 255.255.0.0         |
| C      | 192-223             | 255.255.255.0       |

Subnet mask can be expressed as CIDR (Classless Inter-domain routing), slash notation: 192.168.1.0/24