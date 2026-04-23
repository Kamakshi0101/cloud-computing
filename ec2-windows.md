# Launch EC2 Instance (Windows AMI) on AWS

## Prerequisites
- AWS account (Amazon Web Services)
- Basic understanding of cloud computing
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
- Example: windows-server-1  

### 3.2 Choose AMI
- Select one of the following:
  - Microsoft Windows Server 2019 Base  
  - Microsoft Windows Server 2022 Base  

---

## Step 4: Select Instance Type
- Choose t2.micro (free tier eligible)

---

<img width="1919" height="863" alt="Screenshot 2026-04-23 223021" src="https://github.com/user-attachments/assets/6e70e5c4-245b-4842-a16f-3e0cf030469f" />

## Step 5: Create or Select Key Pair
1. Click "Create new key pair"  
2. Select:
   - Type: RSA  
   - Format: .pem  
3. Download the key file and store it securely  

---

## Step 6: Configure Network Settings
- Allow RDP (Port 3389)
- Source: My IP (recommended for security)

---

## Step 7: Configure Storage
- Default: 30 GB (required for Windows)

---

## Step 8: Launch Instance
- Click "Launch Instance"  
- Wait until the instance state shows "Running"  

---

<img width="1919" height="829" alt="Screenshot 2026-04-23 223051" src="https://github.com/user-attachments/assets/e3b4010e-762a-4a8e-8c4e-53686e2e552d" />

## Step 9: Connect to the Instance

### 9.1 Retrieve Administrator Password
1. Select the instance  
2. Click "Connect"  
3. Go to "RDP Client" tab  
4. Click "Get Password"  
5. Upload the .pem key file  
6. Decrypt the password  

---

### 9.2 Connect via Remote Desktop
1. Copy the Public IP address  
2. Open Remote Desktop Connection  
3. Enter the IP address  
4. Username: Administrator  
5. Paste the decrypted password  

---

## Step 10: Verify Setup
- Access the Windows Server interface  
- Test basic functionality such as browser and system settings  

---

## Step 11: Stop or Terminate Instance
- To avoid charges:
  - Stop: temporary shutdown  
  - Terminate: permanent deletion  

---

## Notes
- Windows instances take longer to initialize compared to Linux  
- Always restrict RDP access for security  
- Free tier eligibility depends on account status  

---

## Summary
- Launched a Windows EC2 instance  
- Connected using Remote Desktop  
- Accessed and verified Windows Server environment  
