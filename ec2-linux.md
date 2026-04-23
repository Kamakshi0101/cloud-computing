
## Prerequisites
- AWS account (Amazon Web Services)
- Basic understanding of cloud computing and SSH
- Key pair for secure access

---

## Step 1: Login to AWS Console
1. Open the AWS Management Console  
2. Sign in with your credentials  
3. Search for "EC2" in the services section  

---

## Step 2: Open EC2 Dashboard
1. Click on "EC2"  
2. Select "Launch Instance"  

---

## Step 3: Configure Instance Details

### 3.1 Name the Instance
- Example: linux-server-1  

### 3.2 Choose AMI
- Select one of the following:
  - Amazon Linux 2  
  - Ubuntu Server 22.04 LTS  

---

<img width="1919" height="770" alt="Screenshot 2026-04-23 223425" src="https://github.com/user-attachments/assets/03bae39e-8a22-4531-918c-10fe3b835919" />


## Step 4: Select Instance Type
- Choose t2.micro (free tier eligible)

---

## Step 5: Create or Select Key Pair
1. Click "Create new key pair"  
2. Select:
   - Type: RSA  
   - Format: .pem  
3. Download the key file and store it securely  

---
<img width="1919" height="828" alt="Screenshot 2026-04-23 223440" src="https://github.com/user-attachments/assets/c7c98d12-fda4-4418-b2c6-a0736b065709" />


## Step 6: Configure Network Settings
- Allow SSH (Port 22)  
- Source: My IP (recommended for security)

---
<img width="1919" height="859" alt="Screenshot 2026-04-23 223533" src="https://github.com/user-attachments/assets/b5d9b283-7b80-4ac3-b14f-901aad4bf0a8" />




## Step 7: Configure Storage
- Default: 8 GB (sufficient for basic usage)

---

## Step 8: Launch Instance
- Click "Launch Instance"  
- Wait until the instance state shows "Running"  

---

<img width="1919" height="967" alt="Screenshot 2026-04-23 223649" src="https://github.com/user-attachments/assets/61a8362b-5951-48d8-a4ff-14c5b1190a73" />


## Step 9: Connect to the Instance

### 9.1 Set Permissions for Key (Windows PowerShell / Git Bash)
```bash
chmod 400 your-key.pem
