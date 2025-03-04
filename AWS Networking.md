## Key VPC Concepts

# IP Address

An IP address is a unique identifier assigned to devices on a network.
It enables devices to communicate with each other using the Internet Protocol (IP).
In AWS, both IPv4 (32-bit) and IPv6 (128-bit) addresses are supported.
An example of an IPv4 address is 192.168.0.1.

# CIDRs (Classless Inter-Domain Routing)

CIDR (Classless Inter-Domain Routing) is a method used to allocate and manage IP addresses.
It allows for flexible allocation of IP address ranges by combining the IP address and the number of significant bits.
CIDR notation is commonly used to represent IP address ranges.
For example, 192.168.0.0/24 represents a range of IP addresses.

# VPC (Virtual Private Cloud

A Virtual Private Cloud (VPC) is a logically isolated virtual network within AWS.
It enables you to define your IP address range, subnets, route tables, and network gateways.
VPC provides control over network configuration and allows you to connect your resources securely.
Creating a VPC involves specifying a name and an IPv4 CIDR block, such as "my-vpc" with CIDR block 10.0.0.0/16.

# Subnets (Private and Public)

Subnets are segments of a VPC's IP address range.
They are used to partition the VPC into smaller networks.
Subnets can be either public or private.
Public subnets have a route to an internet gateway and can be accessed directly from the internet.
Private subnets are not directly accessible from the internet.
Creating subnets involves defining a CIDR block and associating them with the VPC.

# Route Tables

Route tables determine how traffic is directed within a VPC.
They contain rules for routing traffic between subnets, the internet, and other network gateways.
Each subnet is associated with a route table.
Routes are defined to specify where traffic should be directed.
Modifying route tables allows you to control the flow of traffic within your VPC.

# Internet Gateways

Internet gateways enable communication between a VPC and the internet.
They serve as an entry and exit point for internet traffic.
Instances in public subnets can have outbound and inbound internet connectivity through an internet gateway.
Attaching an internet gateway to a VPC and updating the route table to direct internet-bound traffic through the gateway is necessary to enable internet access for resources.

# NAT Gateways

NAT (Network Address Translation) gateways allow instances in private subnets to access the internet.
They provide outbound-only internet connectivity for instances by translating their private IP addresses to public IP addresses.
NAT gateways are used to allow private instances to communicate with the internet while preventing inbound connections from the internet to the instances.

# VPC Flow Logs

VPC Flow Logs capture information about IP traffic going in and out of a VPC.
They provide visibility into network traffic patterns and can be used for troubleshooting and monitoring purposes.
By enabling VPC Flow Logs, you can monitor and analyze network traffic within your VPC.
The logs can be stored in Amazon S3 for further analysis.

# Network Access Control Lists (NACLs)

Network Access Control Lists (NACLs) are stateless firewalls that control inbound and outbound traffic at the subnet level.
They act as a security layer for your VPC and allow or deny traffic based on IP addresses, ports, and protocols.
NACLs are associated with subnets and provide an additional layer of network security for controlling traffic flow.

# Security Groups

Security Groups act as virtual firewalls that control inbound and outbound traffic at the instance level.
They regulate traffic based on security rules defined by the administrator.
Security Groups are associated with instances and provide fine-grained control over traffic flow.
They allow you to specify the protocols, ports, and IP addresses that are allowed to access instances.

# Peering Connections

Peering connections allow VPCs to communicate with each other using private IP addresses.
Peering can be established between VPCs in the same AWS account or different accounts.
It enables the direct routing of traffic between VPCs without going over the internet.
Peering connections are useful for creating network architectures with multiple VPCs.

# Transit Gateways

Transit Gateways enable connectivity between multiple VPCs and on-premises networks.
They act as a central hub for routing traffic between connected networks.
Transit Gateways simplify the network architecture by eliminating the need for point-to-point connections between VPCs.
They provide a scalable and efficient way to connect multiple networks.
