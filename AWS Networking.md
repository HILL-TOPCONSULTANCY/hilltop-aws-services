# **Understanding and Configuring AWS VPC (Virtual Private Cloud)**  

## **Introduction**  
AWS **Virtual Private Cloud (VPC)** is a service that allows you to launch **AWS resources in a logically isolated network**. It provides complete control over networking, including **IP addressing, subnets, route tables, and security policies**.  

You'll learn:  
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

2 ( 32 − subnet bits ) − 2

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
### **Steps to Create a VPC with Public and Private Subnets and Attach EC2 Instances**

---

## **1️⃣ Create the VPC**
1. **Open AWS Console** → Go to **VPC**.
2. Click **Create VPC**.
3. **Enter VPC Name:** e.g., `MyVPC`
4. **IPv4 CIDR Block:** `10.0.0.0/20` (supports ~4,000 IPs).
5. **Tenancy:** Default.
6. Click **Create VPC**.

---

## **2️⃣ Create Two Public and Two Private Subnets**
1. **Go to VPC** → Click **Subnets** → **Create Subnet**.
2. Select **MyVPC**.
3. **Create Public Subnets:**
   - **Public Subnet 1:** `10.0.0.0/22` (AZ: `us-east-1a`)
   - **Public Subnet 2:** `10.0.4.0/22` (AZ: `us-east-1b`)
4. **Create Private Subnets:**
   - **Private Subnet 1:** `10.0.8.0/22` (AZ: `us-east-1a`)
   - **Private Subnet 2:** `10.0.12.0/22` (AZ: `us-east-1b`)
5. Enable **Auto-Assign Public IP** for **public subnets only**.
6. Click **Create Subnets**.

---

## **3️⃣ Create and Attach an Internet Gateway (IGW) for Public Subnets**
1. **Go to VPC** → **Internet Gateways** → Click **Create Internet Gateway**.
2. Enter a **name** (e.g., `MyIGW`).
3. Click **Attach to VPC** → Select **MyVPC** → Attach.

---

## **4️⃣ Create Route Tables for Public and Private Subnets**
1. **Go to VPC** → **Route Tables**.
2. **Create a Route Table for Public Subnets**:
   - Click **Create Route Table** → Name: `Public-RT`
   - **Attach to MyVPC**
   - Click **Create**.
   - Click the new **Route Table**, go to **Routes**, and **Edit Routes**:
     - **Destination:** `0.0.0.0/0`
     - **Target:** Select **Internet Gateway (MyIGW)**
   - Click **Save changes**.
   - Go to **Subnet Associations** → Select **Public Subnet 1 & 2** → Save.

3. **Create a Route Table for Private Subnets**:
   - Click **Create Route Table** → Name: `Private-RT`
   - **Attach to MyVPC**
   - Click **Create**.
   - Go to **Subnet Associations** → Select **Private Subnet 1 & 2** → Save.

---

## **5️⃣ Create and Attach a NAT Gateway for Private Subnets**
1. **Go to VPC** → **NAT Gateways** → **Create NAT Gateway**.
2. Select **Public Subnet 1**.
3. Allocate a **new Elastic IP**.
4. Click **Create**.
5. Go back to **Private Route Table (Private-RT)**:
   - Click **Edit Routes**.
   - Add a route:
     - **Destination:** `0.0.0.0/0`
     - **Target:** **NAT Gateway**
   - Click **Save changes**.

---

## **6️⃣ Launch EC2 Instances**
### **Launch a Public Instance**
1. **Go to EC2** → Click **Launch Instance**.
2. Select **Amazon Linux 2** or **Ubuntu**.
3. **Instance Type:** `t2.micro` (free tier).
4. **Network:** Select `MyVPC`.
5. **Subnet:** Choose **Public Subnet 1**.
6. **Enable Auto-Assign Public IP**.
7. **Security Group:** Allow **SSH (22)** from `0.0.0.0/0` for testing.
8. **Key Pair:** Create a new key pair (`my-key.pem`).
9. Click **Launch**.

### **Launch a Private Instance**
1. **Repeat the above steps**, but:
   - **Choose Private Subnet 1**.
   - **DO NOT** enable **Auto-Assign Public IP**.
   - **Security Group:** Allow **SSH (22) only from the public instance** (e.g., `10.0.0.0/20`).
   - Click **Launch**.

---

## **7️⃣ Testing the Setup**
### **Access the Public Instance**
1. Open **Terminal (Mac/Linux) or Git Bash (Windows)**.
2. Connect to the public instance:
   ```sh
   ssh -i my-key.pem ec2-user@<PUBLIC_IP>
   ```
3. Once connected, get the private IP:
   ```sh
   ip a
   ```

### **Access the Private Instance (Using Public Instance as a Bastion)**
1. **From the Public EC2 Instance**, SSH into the Private EC2:
   ```sh
   ssh -i my-key.pem ec2-user@<PRIVATE_IP>
   ```
2. You should now be inside the private instance.


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

## **4️⃣ Best Practices for AWS VPC Security**  
✅ **Use least privilege access in Security Groups**  
✅ **Block SSH/RDP access from the public internet**  
✅ **Use AWS VPC Flow Logs for traffic monitoring**  
✅ **Enable Encryption for sensitive data in RDS or S3**  
✅ **Use Multi-AZ deployments for high availability**  
✅ **Implement private endpoints for secure service access**  

---
