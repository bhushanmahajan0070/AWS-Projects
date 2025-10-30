# 🌐 Custom VPC Setup on AWS

## 📘 Project Overview
In this project, I created a **Custom VPC (Virtual Private Cloud)** on AWS from scratch using the AWS Management Console.  
This project helped me understand how networking works in AWS — how subnets, route tables, gateways, and security groups connect together to build a secure cloud environment.

---

## 🧠 What is a VPC?
**Amazon VPC (Virtual Private Cloud)** is your own private network within the AWS Cloud.  
It gives you full control over your IP range, subnets, route tables, and gateways to securely connect your AWS resources.

---

## ⚙️ Step-by-Step Setup Guide

### 🪄 Step 1: Login to AWS Console
1. Open [AWS Management Console](https://aws.amazon.com/console/)
2. In the search bar, type **VPC** and open the **VPC Dashboard**

---

### 🧱 Step 2: Create a New VPC
1. Click **Create VPC**
2. Choose **VPC Only** or **VPC and more** (recommended)
3. Enter:
   - **Name tag:** `MyCustomVPC`
   - **IPv4 CIDR block:** `10.0.0.0/16`  
     *(This defines your private IP range — you can use any range like 172.16.0.0/16, etc.)*
   - **Tenancy:** Default
4. Click **Create VPC**
5. ✅ You’ve now created a new virtual network!

---

### 🌍 Step 3: Create Subnets
Now we’ll divide the VPC into smaller sections — **Public** and **Private Subnets**.

#### ➕ Create Public Subnet
1. In the left panel, click **Subnets → Create Subnet**
2. Select **VPC:** `MyCustomVPC`
3. Enter:
   - **Subnet name:** `Public-Subnet-1`
   - **Availability Zone:** `ap-south-1a` (or any region zone)
   - **IPv4 CIDR block:** `10.0.1.0/24`
4. Click **Create Subnet**

#### ➕ Create Private Subnet
1. Click **Create Subnet** again
2. Select **VPC:** `MyCustomVPC`
3. Enter:
   - **Subnet name:** `Private-Subnet-1`
   - **Availability Zone:** `ap-south-1b`
   - **IPv4 CIDR block:** `10.0.2.0/24`
4. Click **Create Subnet**

✅ Now your VPC has 2 subnets — one public, one private.

---

### 🌐 Step 4: Create and Attach an Internet Gateway (IGW)
An **Internet Gateway** allows instances in the public subnet to connect to the Internet.

1. In the left menu, click **Internet Gateways**
2. Click **Create Internet Gateway**
   - **Name:** `MyVPC-IGW`
3. Click **Create Internet Gateway**
4. Select the newly created IGW → Click **Actions → Attach to VPC**
5. Choose `MyCustomVPC` and click **Attach**

✅ Now your VPC has internet connectivity capability.

---

### 🛣️ Step 5: Create Route Tables
A **Route Table** defines how traffic moves between your subnets, internet, and gateways.

#### 🗺️ Public Route Table
1. Click **Route Tables → Create Route Table**
2. Name: `Public-RT`
3. Select VPC: `MyCustomVPC`
4. Click **Create Route Table**

#### Add Route to Internet
1. Select `Public-RT` → Click **Routes → Edit routes**
2. Click **Add route**
   - **Destination:** `0.0.0.0/0`
   - **Target:** Select your **Internet Gateway (MyVPC-IGW)**
3. Click **Save changes**

#### Associate Public Subnet
1. In `Public-RT`, go to **Subnet associations → Edit subnet associations**
2. Select **Public-Subnet-1**
3. Click **Save associations**

✅ Your public subnet can now reach the internet.

---

### 🔒 Step 6: Create a NAT Gateway (Optional for Private Subnet)
If your private subnet instances need to access the internet (for updates), use a **NAT Gateway**.

1. Go to **NAT Gateways → Create NAT Gateway**
2. Choose:
   - **Subnet:** `Public-Subnet-1`
   - **Elastic IP:** Click **Allocate Elastic IP**
3. Click **Create NAT Gateway**

#### Create Private Route Table
1. Go to **Route Tables → Create Route Table**
   - Name: `Private-RT`
   - VPC: `MyCustomVPC`
2. Click **Create Route Table**

#### Add Route via NAT
1. Select `Private-RT` → Routes → Edit Routes
2. Add Route:
   - **Destination:** `0.0.0.0/0`
   - **Target:** Select your **NAT Gateway**
3. Save changes

#### Associate Private Subnet
1. Go to **Subnet associations → Edit subnet associations**
2. Select **Private-Subnet-1**
3. Click **Save**

✅ Now your private subnet can access the internet **only through NAT**, safely.

---

### 🧰 Step 7: Configure Security Groups
1. Go to **Security Groups → Create Security Group**
   - **Name:** `MyVPC-SG`
   - **VPC:** `MyCustomVPC`
   - **Description:** Allow SSH and HTTP
2. Add inbound rules:
   - Type: **SSH** | Port: `22` | Source: `My IP`
   - Type: **HTTP** | Port: `80` | Source: `0.0.0.0/0`
3. Save Security Group

---

### 🖥️ Step 8: Launch an EC2 Instance in Public Subnet
1. Go to **EC2 → Launch Instance**
2. Name: `WebServer`
3. AMI: **Amazon Linux 2**
4. Instance type: `t2.micro`
5. Key pair: Choose existing or create new
6. Network Settings:
   - VPC: `MyCustomVPC`
   - Subnet: `Public-Subnet-1`
   - Auto-assign Public IP: **Enable**
   - Security Group: Select `MyVPC-SG`
7. Click **Launch Instance**

✅ Your EC2 instance is now running in your custom VPC.

---

### 🌎 Step 9: Connect to EC2 Instance
1. Select your instance → Click **Connect**
2. Use the SSH command:
   ```bash
   ssh -i "your-key.pem" ec2-user@<public-ip>
