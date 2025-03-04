# **Tutorial: Understanding and Configuring AWS VPC (Virtual Private Cloud)**  

## **Introduction**  
AWS **Virtual Private Cloud (VPC)** is a service that allows you to launch **AWS resources in a logically isolated network**. It provides complete control over networking, including **IP addressing, subnets, route tables, and security policies**.  

In this tutorial, you'll learn:  
✅ What is AWS VPC?  
✅ Key components of a VPC  
✅ How to create and configure a VPC in AWS  
✅ Best practices for securing a VPC  

---

## **1️⃣ What is AWS VPC?**  
AWS **VPC (Virtual Private Cloud)** enables you to create a **private, isolated network** within AWS. You can:  
- **Define custom IP address ranges**  
- **Control inbound/outbound traffic**  
- **Connect securely to on-premises data centers**  
- **Segment resources using subnets**  

By default, AWS provides a **default VPC**, but you can create a **custom VPC** for better security and network control.  

---

## **2️⃣ Key Components of AWS VPC**  
### **1. VPC (Virtual Private Cloud)**  
A VPC is a **virtual network** where you can run AWS resources. Each VPC has:  
- **IPv4 CIDR block** (e.g., `10.0.0.0/16`)  
- (Optional) **IPv6 CIDR block**  

# **Understanding IP Address Classes and CIDR Notation Calculation**  

## **1️⃣ Introduction to IP Addressing**  
An **IP address** is a unique identifier assigned to devices on a network. It consists of **four octets** (8-bit numbers) separated by dots, such as `192.168.1.1`.  

Each octet can range from **0 to 255** (since 8 bits = 2⁸ = 256 possible values).  

There are two types of IP addresses:  
- **IPv4 (32-bit)** → `192.168.1.1`  
- **IPv6 (128-bit)** → `2001:db8::ff00:42:8329`  

---

## **2️⃣ Different IP Address Classes**  
IPv4 addresses are divided into **five classes (A, B, C, D, E)** based on the first octet.

### **Class A (1.0.0.0 – 126.255.255.255)**  
- **First Octet Range:** `1 - 126`  
- **Subnet Mask (Default):** `255.0.0.0 (/8)`  
- **Supports:** Large networks (up to 16 million hosts per network)  
- **Example:** `10.0.0.1`  

### **Class B (128.0.0.0 – 191.255.255.255)**  
- **First Octet Range:** `128 - 191`  
- **Subnet Mask (Default):** `255.255.0.0 (/16)`  
- **Supports:** Medium-sized networks (up to 65,000 hosts per network)  
- **Example:** `172.16.0.1`  

### **Class C (192.0.0.0 – 223.255.255.255)**  
- **First Octet Range:** `192 - 223`  
- **Subnet Mask (Default):** `255.255.255.0 (/24)`  
- **Supports:** Small networks (up to 254 hosts per network)  
- **Example:** `192.168.1.1`  

### **Class D (224.0.0.0 – 239.255.255.255) (Multicast)**  
- Used for **multicast traffic** (not for individual hosts).  
- Example: `239.1.1.1`  

### **Class E (240.0.0.0 – 255.255.255.255) (Reserved)**  
- Reserved for future use or research.  

---

## **3️⃣ CIDR Notation (Classless Inter-Domain Routing)**  
CIDR notation is a method for **subnetting** and **IP address allocation**.  

### **Understanding CIDR Notation (`/n`)**
CIDR notation represents the **number of bits used for the network portion** of an IP address.  

For example:  
- `192.168.1.0/24` → **First 24 bits (3 octets) represent the network**, last 8 bits are for hosts.  
- `10.0.0.0/16` → **First 16 bits (2 octets) represent the network**, last 16 bits are for hosts.  

### **Subnet Mask to CIDR Table**
| **CIDR Notation** | **Subnet Mask**      | **Hosts per Subnet** |
|------------------|-------------------|-------------------|
| `/8`   | `255.0.0.0`   | 16,777,214 hosts |
| `/16`  | `255.255.0.0` | 65,534 hosts |
| `/24`  | `255.255.255.0` | 254 hosts |
| `/30`  | `255.255.255.252` | 2 hosts |

Formula to calculate hosts per subnet:

2
(
32
−
subnet bits
)
−
2
2 
(32−subnet bits)
 −2
> (Subtract **2** for **network & broadcast addresses**)

---

## **4️⃣ CIDR Calculation Examples**
### **Example 1: `192.168.1.0/24`**
- **Subnet Mask:** `255.255.255.0`
- **Hosts per Subnet:** `2^(32 - 24) - 2 = 254`
- **Network ID:** `192.168.1.0`
- **Broadcast Address:** `192.168.1.255`
- **Valid Host Range:** `192.168.1.1` → `192.168.1.254`

---

### **Example 2: `10.0.0.0/16`**
- **Subnet Mask:** `255.255.0.0`
- **Hosts per Subnet:** `2^(32 - 16) - 2 = 65,534`
- **Network ID:** `10.0.0.0`
- **Broadcast Address:** `10.0.255.255`
- **Valid Host Range:** `10.0.0.1` → `10.0.255.254`

---

## **5️⃣ Subnetting Example**
Let’s say you need **four subnets** from `192.168.1.0/24`.

### **Subnet Calculation:**
- **Original Network:** `192.168.1.0/24`  
- **New Subnet Mask:** `/26` (`255.255.255.192`)  
- **New Subnet Blocks:** `64` (because `256/4 = 64`)  
- **New Subnets:**  
  - `192.168.1.0/26` → Hosts: `192.168.1.1 - 192.168.1.62`
  - `192.168.1.64/26` → Hosts: `192.168.1.65 - 192.168.1.126`
  - `192.168.1.128/26` → Hosts: `192.168.1.129 - 192.168.1.190`
  - `192.168.1.192/26` → Hosts: `192.168.1.193 - 192.168.1.254`

---

## **3️⃣ How to Create a Custom VPC in AWS**  
### **Step 1: Log in to AWS and Navigate to VPC**  
1. Sign in to the **AWS Management Console**.  
2. Go to **VPC Dashboard** → Click **Create VPC**.  

### **Step 2: Define Your VPC**  
1. **VPC Name**: `MyCustomVPC`  
2. **IPv4 CIDR Block**: `10.0.0.0/16`  
3. **Enable DNS Support**: ✅  
4. Click **Create VPC**.  

### **Step 3: Create Subnets**  
1. Go to **Subnets** → Click **Create Subnet**.  
2. Select **MyCustomVPC** and create the following subnets:  
   - **Public Subnet** → `10.0.1.0/24`  
   - **Private Subnet** → `10.0.2.0/24`  
   - **Database Subnet** → `10.0.3.0/24`  
3. Click **Create**.  

### **Step 4: Create an Internet Gateway (IGW)**  
1. Go to **Internet Gateways** → Click **Create IGW**.  
2. Name it `MyCustomIGW` → Attach it to `MyCustomVPC`.  

### **Step 5: Configure Route Tables**  
1. Go to **Route Tables** → Click **Create Route Table**.  
2. Create two route tables:  
   - **Public Route Table** (Associates with the Public Subnet)  
   - **Private Route Table** (Associates with the Private & Database Subnets)  
3. Add a **route for the Public Route Table**:  
   - **Destination**: `0.0.0.0/0` → **Target**: Internet Gateway  

### **Step 6: Create a NAT Gateway (Optional - For Private Subnet Internet Access)**  
1. Go to **NAT Gateways** → Click **Create NAT Gateway**.  
2. Attach it to **Public Subnet** and **Elastic IP**.  
3. Update the **Private Route Table**:  
   - **Destination**: `0.0.0.0/0` → **Target**: NAT Gateway  


--- 

### **3. Internet Gateway (IGW)**  
The **Internet Gateway (IGW)** allows public-facing resources (like web servers) to access the internet.  

### **4. NAT Gateway**  
The **NAT Gateway** allows private subnets to access the internet **without exposing them** directly.  

### **5. Route Tables**  
**Route tables** define how traffic is directed within a VPC.  
- **Public route table** → Sends traffic through the **Internet Gateway**  
- **Private route table** → Routes traffic internally or through a **NAT Gateway**  

### **6. Security Groups & NACLs (Network ACLs)**  
- **Security Groups** → Acts as a **firewall** for EC2 instances. Controls **inbound & outbound traffic**.  
- **Network ACLs (NACLs)** → Controls traffic at the **subnet level** (stateless filtering).  

### **7. VPC Peering & VPN**  
- **VPC Peering** allows communication between **two VPCs**.  
- **AWS VPN** allows a **secure connection to on-premises networks**.  

---

## **3️⃣ How to Create a Custom VPC in AWS**  
### **Step 1: Log in to AWS and Navigate to VPC**  
1. Sign in to the **AWS Management Console**.  
2. Go to **VPC Dashboard** → Click **Create VPC**.  

### **Step 2: Define Your VPC**  
1. **VPC Name**: `MyCustomVPC`  
2. **IPv4 CIDR Block**: `10.0.0.0/16`  
3. **Enable DNS Support**: ✅  
4. Click **Create VPC**.  

### **Step 3: Create Subnets**  
1. Go to **Subnets** → Click **Create Subnet**.  
2. Select **MyCustomVPC** and create the following subnets:  
   - **Public Subnet** → `10.0.1.0/24`  
   - **Private Subnet** → `10.0.2.0/24`  
   - **Database Subnet** → `10.0.3.0/24`  
3. Click **Create**.  

### **Step 4: Create an Internet Gateway (IGW)**  
1. Go to **Internet Gateways** → Click **Create IGW**.  
2. Name it `MyCustomIGW` → Attach it to `MyCustomVPC`.  

### **Step 5: Configure Route Tables**  
1. Go to **Route Tables** → Click **Create Route Table**.  
2. Create two route tables:  
   - **Public Route Table** (Associates with the Public Subnet)  
   - **Private Route Table** (Associates with the Private & Database Subnets)  
3. Add a **route for the Public Route Table**:  
   - **Destination**: `0.0.0.0/0` → **Target**: Internet Gateway  

### **Step 6: Create a NAT Gateway (Optional - For Private Subnet Internet Access)**  
1. Go to **NAT Gateways** → Click **Create NAT Gateway**.  
2. Attach it to **Public Subnet** and **Elastic IP**.  
3. Update the **Private Route Table**:  
   - **Destination**: `0.0.0.0/0` → **Target**: NAT Gateway  

---

## **4️⃣ Best Practices for AWS VPC Security**  
✅ **Use least privilege access in Security Groups**  
✅ **Block SSH/RDP access from the public internet**  
✅ **Use AWS VPC Flow Logs for traffic monitoring**  
✅ **Enable Encryption for sensitive data in RDS or S3**  
✅ **Use Multi-AZ deployments for high availability**  
✅ **Implement private endpoints for secure service access**  

---
