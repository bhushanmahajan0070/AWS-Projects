# 🚀 Launching an Amazon EC2 Instance on AWS

## 📘 Project Overview
In this project, I launched an **Amazon EC2 instance** (Elastic Compute Cloud) — a virtual server in the AWS cloud.  
This marks an important step in my AWS learning journey, where I learned how to set up and access a cloud-based virtual machine.

---

## 🧠 What is EC2?
**Amazon EC2 (Elastic Compute Cloud)** allows you to run virtual servers in the cloud.  
You can use EC2 to deploy websites, host apps, or test software in a secure and scalable way.

---

## ⚙️ Steps to Launch an EC2 Instance

### 🪄 Step 1: Login to AWS Console
- Go to [AWS Management Console](https://aws.amazon.com/console/)
- Search for **EC2**
- Click on **EC2 Dashboard**

---

### 🧩 Step 2: Create a New Instance
- Click **Launch Instance**
- Enter a **Name** for your instance (e.g., `MyFirstEC2`)
- Choose an **Amazon Machine Image (AMI)**  
  Example: `Amazon Linux 2 AMI (Free Tier Eligible)`
- Choose **Instance Type**  
  Example: `t2.micro` (Free Tier Eligible)
- Choose or **Create a Key Pair**
  - Click “Create new key pair”
  - Download the `.pem` file (keep it safe — you’ll need it to connect)
- Configure **Network Settings**
  - Select **VPC**
  - Enable **Auto-assign Public IP**
  - Choose an existing **Security Group** or create a new one
    - Allow **SSH (port 22)** for Linux
    - Allow **HTTP (port 80)** if hosting a website
- Click **Launch Instance**

---

### 💻 Step 3: Connect to Your EC2 Instance (Using SSH)
Once the instance is running:

1. Select your instance → Click **Connect**
2. Copy the SSH command (example below)
   ```bash
   ssh -i "your-key.pem" ec2-user@<your-public-ip>
