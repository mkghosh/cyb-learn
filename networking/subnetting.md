# Subnetting

## What is subnetting?

The division of an address range of IPv4 addresses into several smaller ranges is called subnetting.

A subnet is a logical segment of a network that uses IP addresses with the same network address. We can think of a subnet as a labeled entrance on a large building corridor. For example, this could be a glass door that separates various departments of a company building. With the help of subnetting, we can create a specific subnet by ourselves or find out the following outline of the respective network:

- Network address
- Broadcast address
- First host
- Last host
- Number of hosts

Let us take the following IPv4 address and subnet mask as an example:

- IPv4 Address: 192.168.12.160
- Subnet Mask: 255.255.255.192
- CIDR: 192.168.12.160/26

We already know that an IP address is divided into the **network part** and the **host part**.

## Network Part

|Details of|1st Octet|2nd Octet|3rd Octet|4th Octet|Decimal
|---|---|---|---|---|---
|IPv4|1100 0000|1010 1000|0000 1100|1010 0000|192.168.12.160/26
|Subnet Mask|1111 1111|1111 1111|1111 1111|1100 0000|255.255.255.192
|Bits|/8|/16|/24|/32|-

In subnetting, we use subnet mask as a template for the IPv4 address. From the 1-bits in the subnet mask, we know which bits in the IPv4 address cannot be changed. These are fixed and therefore determine the main network in which the subnet is located.

The bits in the host part can be changed to the first and last address. The first address is the network address, and the last address is the broadcast address for the respective subnet.

The network address is vital for the delivery of a data packet. If the network address is the same for the source and destination address, the data packet is delivered within the same subnet. If the network addresses are different, the data packet must be routed to another subnet via the default gateway.

The subnet mask determines where this separation occurs. If we set all bits in the host part of the IPv4 to 0, we get the respective subnet's network address, and if we set all bits of the host part to 1, we get the broadcast address of that subnet.

In this case the network address becomes:

```txt
IPv4 = 1100 0000 1010 1000 0000 1100 10|00 0000 192.168.12.128/26
Netmask = 1111 1111 1111 1111 1111 1111 11|00 0000 255.255.255.192
```

Since we now know that the IPv4 addresses 192.168.12.128 and 192.168.12.191 are assigned, all other IPv4 addresses are accordingly between 192.168.12.129-190. Now we know that this subnet offers us a total of 64 - 2 (network and broadcast address) or 62 IPv4 addresses that we can assign to our hosts.

|Hosts|IPv4
|---|---
|Network Address|192.168.12.128
|First Host|192.168.12.129
|Other Hosts|...
|Last Host|192.168.12.190
|Broadcast Address|192.168.12.191
