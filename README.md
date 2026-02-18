# EC2 Ubuntu Setup Guide

This repository provides clear, step-by-step instructions for creating, configuring, and securing an **Ubuntu EC2 instance** on Amazon Web Services (AWS). It is designed for students, developers, and administrators who want a reliable and repeatable setup process.

---

## Table of Contents
- [Prerequisites](#prerequisites)
- [1.Launch an EC2 Ubuntu Instance](#1launch-an-ec2-ubuntu-instance)
  - [Step 1 — Open EC2 Dashboard](#step-1--open-ec2-dashboard)
  - [Step 2 — Configure Instance](#step-2--configure-instance)
  - [Step 3 — Storage](#step-3--storage)
  - [Step 4 — Launch](#step-4--launch)
- [2.Connect to Your Instance](#2connect-to-your-instance)
  - [Find Public IP](#find-public-ip)
  - [SSH Access](#ssh-access)
- [3.Initial Server Configuration](#️3-initial-server-configuration)
- [4.Security Hardening](#4security-hardening)
- [Optional](#optional)

---

## 📌Prerequisites

Before starting, ensure you have:

- An active **AWS account**
- IAM user with permissions for EC2, VPC, and Key Pair creation
- SSH client (Linux/macOS) or PuTTY (Windows)
- Basic Linux command-line knowledge

---

## 🚀1.Launch an EC2 Ubuntu Instance

### Step 1 — Open EC2 Dashboard
1. Log in to the AWS Console  
2. Search for **EC2**  
3. Select **Launch Instance**

### Step 2 — Configure Instance
| Setting | Value |
|--------|--------|
| **Name** | ubuntu-server (or your preferred name) |
| **AMI** | Ubuntu Server 22.04 LTS |
| **Instance Type** | t2.micro (Free Tier eligible) |
| **Key Pair** | Create new or use existing |
| **Network** | Default VPC or custom |
| **Security Group** | Allow SSH (port 22) |

### Step 3 — Storage
- Default 8GB is usually enough  
- Increase if your project requires more space

### Step 4 — Launch
Click **Launch Instance** and wait for initialization.

---

## 🔑2.Connect to Your Instance

### Find Public IP
- Go to **EC2 → Instances**
- Select your instance
- Copy the **Public IPv4 address**

### SSH Access

#### Linux/macOS
```bash
chmod 400 your-key.pem
ssh -i your-key.pem ubuntu@<PUBLIC-IP>
```
---

## 🛠️3.Initial Server Configuration

Update Packages:
```bash
sudo apt update && sudo apt upgrade -y
```
Install Basic Tools:
```bash
sudo apt install -y git curl unzip htop
```
(Optional) Create a New User
```bash
sudo adduser <username>
sudo usermod -aG sudo <username>
```
---

## 🔐4.Security Hardening

Enable UFW Firewall:
```bash
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw status
```
Disable Root SSH Login (Optional):
```bash
sudo nano /etc/ssh/sshd_config
```
Restart SSH:
```bash
sudo systemctl restart ssh
```
---

## Optional 

Install Common Server Software

Install Docker
```bash
sudo apt install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
sudo usermod -aG docker ubuntu
```
