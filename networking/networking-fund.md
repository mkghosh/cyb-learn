# DNS Details

- DNS stands for Domain Name System.

It provides a simple way for us to communicate with devices on the internet without remembering complex numbers. Much like every house has a unique address for sending mail directly to it, every computer on the internet has its own unique address to communicate with it called an IP address. An IP address looks linke the following 104.11.12.203 4 sets of digits ranging from 0-255 separated by a period. When you want to visit a website, it's not exactly convenient to remember this complicated set of numbers, and that's where DNS can help. So instead of remembering IP you can remember the website name. Like instead of 104.26.10.229, you can remember tryhackme.com.

## Domain Hierarchy

## TLD (Top-Level Domain)

A TLD is the most righthand part of a domain name. So, for example, the tryhackme.com TLD is **.com**. There are two types of TLDs.

1. gTLD: Generic Top Level Domain or gTLD was meant to tell the user the domain name's purpose; for example, a .com would be for commercial, .org would be for organization, .edu for education etc.

2. ccTLD: Country Code Top Level Domain is used for geofraphical purposes, for example .ca for Canada, .bd for Bangladesh, .co.uk for United Kingdom etc.

Now a days there is a huge demand for gTLD that is why there are a lot of new gTLD's are available out there. You can find the full list of gTLD's from [here](https://data.iana.org/TLD/tlds-alpha-by-domain.txt)

## Second Level Domain

Taking [tryhackme.com](https://tryhackme.com) as an example, the .com part is the TLD, and tryhackme is the second level domain. When registering a domain name, the second-level domain is limited to 63 characters + the TLD and can only use a-z 0-9 and hyphens (cannot start or end with hyphens or have consecutive hyphens).

## Subdomain

A subdomain sits on the left-hand side of the Second-Level Domain using a period to seperate it; for example, in the name admin.tryhackme.com the admin part is the subdomain. A subdomain name has the same creating restrictions as Second-Level Domain, being limited to 63 characters and can only use a-z 0-9 and hyphens (cannot start or end with hyphens or have consecutive hyphens). You can use multiple subdomains split with periods to create longer names, such as jupiter.servers.tryhackme.com. But the length must be kept to 253 characters or less. There is no limit to the number of subdomains you can create for your domain name.

## DNS record types

DNS isn't just for websites though, and multiple types of DNS record exist. We'll go over some of the most common ones that you're likely to come across.

## A Record

These are records that resolve to IPv4 addresses, for example 101.12.15.13

## AAAA Record

These records resolve to IPv6 addresses, for example 2606:5700:20::682b:be5

## CNAME Record

These records resolve to another domain name, for example tryhackme's online shop has the subdomain store.tryhackme.com which returns a CNAME record shops.shopify.com. Another DNS request would then be made to shops.shopify.com to work out the IP address.

## MX Record

These Records resolve to the address of the servers that handle the email for the domain you are querying, for example an MX record response for tryhackme.com would look something like alt1.aspmx.l.google.com These records also come with a priority flag. This tells the client in which order to try the servers, this is perfect for if the main server goes down and email needs to be sent to a backup server.

## TXT Record

TXT records are free text fields where any text-based data can be stored. TXT records have multiple uses, but some common ones can be to list servers that have the authority to send and email on behalf of the domain (this can help in the battle against spam and spoofed email). They can also be used to verify ownership of the domain name when signing up for third party services.

## Example of DNS query

We can use nslookup to query DNS records of a domain.

- For finding A records
nslookup --type=A website-address

- For CNAME
  nslookup --type=CNAME web-address
and so on.

## Details of HTTP request

- HTTP stands for Hypertext Transfer Protocol. It is used whenever you view a website, developed by Tim Berners-Lee and his team between 1989-1991.
- HTTPS stands for Hypertext Transfer Protocol Secure. It is the secure version of HTTP. HTTPS data is encrypted so it not only stops people from seeing the data you are receiving and sending, but it also gives you assurances that you're talking to the correct web server and not something impersonating it.
- URL stands for Uniform Resource Locator. If you've used the internet, you've used a URL before. A URL is predominantly and instruction on how to access a resource on the internet.

An URL makes up of 8 components

- Scheme: This instructs on what protocol to use for accessing the resource such as HTTP, HTTPS, FTP (File Transfer Protocol).
- User: Some services require authentication to log in, you can put a username and password into the url to log in.
- Host: The domain or IP address of the server you wish to access.
- Port: The port that you are going to connect to, usually 80 for HTTP and 443 for HTTPS, but this can be hosted on any prot between 1-65535.
- Path: The file name or location of the resource you are trying to access.
- Query String: Extra bits of information that can be sent to the requested path, For example, /blog?**id=1** would tell the blog path that you wish to receive the blog article with the id of 1.
- Fragment: This is a reference to a location on the actual page requested. This is commonly used for pages with long content and can have certian part of the page directly linked to it, so it is viewable to the user as soon as they access the page.

## Networking

## What is networking?

- A network is simply two or more computers linked together to share data, information or resources.
- There are two basic types of networks:
  - LAN (Locak area network): A local area netwrok (LAN) is a network typically spanning a single floor or building. This is commonly a limited geographical area.
  - WAN (Wide area network): Wide area network is the term usually assigned to the long-distance connections between geographically remote networks.

## Network devices

- Hubs: Hubs are used to connec multiple devices in a network. They're less likely to be seen in business or corporate networks than in home networks. Hubs are wired devices and are not as smart as switches or routers.
- Switch: Rather that using a hub, you might consider using a switch, or what is also known as an intelligent hub. Switches are wired devices that know the addresses of the devices connected to them and route traffic to that port/device rather than retransmitting to all devices.
Offering greater efficiency for traffic delivery and improving the overall throughput of data, switches are smarter than hubs, but not as smart as routers. Switches can also create separate broadcast domains when used to create VLANs.
- Router: Routers are used to control traffic flow on networks and are often used to connect similar networks and control traffic flow between them. Routers can be wired or wireless and can connect multiple switches. Smarter than hubs and switches, routers determine the most efficient "route" for the traffic to flow across the network.
- Firewall: Firewalls are essential tools in managing and controlling network traffic and protecting the network. A firewall is a network device used to filter traffic. It is typically deployed between a private network and the internet, but it can also be deployed between departments (segmented networks) within an organization (overall network). Firewalls filter traffic based on a defined set of rules, also called filters or access control lists.
- Server: A server is a computer that provides information to other computers on a netwrok. Some common servers are web servers, email servers, print servers, database servers, and file servers. All of these are, by design, networked and accessed in some way by a client computer. Servers are usually secured differently than workstations to protect the information they contain.
- Endpoint: Endpoints are the ends of a network communication link. One end is often at a server where a resource resides, and the other end is often a client making a request to use a network resource. An endpoint can be another server, desktop workstation, laptop, tablet, mobile phone, or any other end user device.

## Microsegmentation Characteristics

- Microsegmentation allows for granular restrictions within the IT environment, to the point where rules can be applied to individual machines and/or users, and these rules can be as detailed and complex as desired. For instance, it can limit which IP addresses can communicate to a given machine, at which time of day, with which credential, and which services those connections can use.
- Microsegmentation uses logical rules, not physical rules, and does not require additional hardware or manual interaction with the device (that is, the administrator can apply the rules to various machines without having to physically touch each device or the cables connecting it to the networked environment).
- Microsegmentation is the ultimate end state of the defense-in-depth philosophy; no single point of access within the IT environment can lead to broader compromise.
- Microsegmentation is crucial in shared environments, such as the cloud, where more than one customer's data and functionality might reside on the same device(s), and where third-party personnel (administrators/technicians who work for the cloud provider, not the customer) might have physical access to the devices.
- Microsegmentation allows the organization to limit which business functions, units, offices, or departments can communicate with others, to enforce the concept of least privilege. For instance, the Human Resources office probably has employee data that no other business unit should have access to, such as employee home address, salary, and medical records. Microsegmentation, like VLANs, can make HR its own distinct IT enclave, so that sensitive data is not available to other business units, thus reducing the risk of exposure.
- In modern environments, microsegmentation is available because of virtualization is available because of virtualization and software-defined networking (SDN) technologies. In the cloud, the tools for applying this strategy are often called "virtual private networks (VPN)" or "security groups."
- Even in your home, microsegmentation can be used to separate computers from smart TVs, air conditioning, and smart appliances, which can be connected and have vulnerabilities
  