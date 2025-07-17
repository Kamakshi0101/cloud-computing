## ✅ Step 1: Launch EC2 Instances Using a Bash Script

1. Go to the [AWS EC2 Console](https://console.aws.amazon.com/ec2)
2. Click on **Launch Instance**
3. Fill in the details:
   - **Name**: `server-1`
   - **Amazon Machine Image (AMI)**: Select **Ubuntu Server 22.04 LTS (HVM), SSD Volume Type**
   - **Instance type**: `t2.micro` (Free Tier eligible)
   - **Key pair**: Create new or select existing
   - **Network Settings**: Use the custom VPC that you created
       - **Type**: HTTP | **Protocol**: TCP | **Port**: 80 | **Source**: Anywhere (0.0.0.0/0)
       - **Type**: SSH | **Protocol**: TCP | **Port**: 22 | **Source**: My IP


<img width="1920" height="1080" alt="Screenshot (173)" src="https://github.com/user-attachments/assets/0e6d7826-166b-45f5-97b6-b853205ddf85" />

### Add User Data Script

In the **Advanced Details** section, paste the following **user-data** script:

```bash
#!/bin/bash
apt-get update
apt-get install nginx -y

cat <<EOF > /var/www/html/index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Welcome to $(hostname)</title>
  <style>
    body {
      background: linear-gradient(135deg, #1e3c72, #2a5298);
      color: white;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }
    h1 {
      font-size: 3em;
      margin: 0.2em 0;
    }
    p {
      font-size: 1.2em;
      color: #cce3ff;
    }
    .hostname {
      background: #ffffff33;
      padding: 0.5em 1em;
      border-radius: 10px;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <h1> Welcome to Kamakshi's Server!</h1>
  <p>This server is proudly hosted as:</p>
  <div class="hostname">$(hostname)</div>
</body>
</html>
EOF
```
<img width="1920" height="1080" alt="Screenshot (174)" src="https://github.com/user-attachments/assets/265e4e30-3275-4d9f-b221-1568e1b5ca48" />

**Similarly create three servers: server-1, server-2 and server-3 respectively**

<img width="1920" height="1080" alt="Screenshot (176)" src="https://github.com/user-attachments/assets/bdd24a34-8db2-46ae-a700-8c2617c0fe53" />
## ✅ Step 2: Create a Target Group and Register EC2 Instances

Once your EC2 instances are launched and NGINX is running on them (port 80), the next step is to create a **Target Group** and register your instances. This Target Group will later be linked with a Load Balancer.

---

### Navigate to Target Groups

1. Go to the **AWS Management Console**
2. Open the **EC2 Dashboard**
3. From the left-hand sidebar, click on **Target Groups** under **Load Balancing**
4. Click **Create target group**


### Configure Target Group

Fill out the target group creation form as follows:

- **Target type**: `Instances`
- **Target group name**: `TargetGroup-1` (or any name you prefer)
- **Protocol**: `HTTP`
- **Port**: `80`
- **VPC**: Select the VPC where your EC2 instances are running
- **Health checks**:
  - **Protocol**: HTTP
  - **Path**: `/`

> Leave other settings as default unless you have specific health check requirements.


### Register Targets (EC2 Instances)

On the **Register targets** page:

1. You’ll see a list of running EC2 instances in the selected VPC.
2. Select the checkboxes for the 3 NGINX EC2 instances you created.
3. Under **Ports for the selected instances**, make sure the port is set to `80`.
4. Click **Include as pending below**.
5. Scroll down and click **Create target group**.

<img width="1920" height="1080" alt="Screenshot (178)" src="https://github.com/user-attachments/assets/8f7a6333-bc00-4936-b636-5411bb599bf0" />

<img width="1920" height="1080" alt="Screenshot (179)" src="https://github.com/user-attachments/assets/83282742-3c96-4af8-b0e2-a6ba38dc2496" />

<img width="1920" height="1080" alt="Screenshot (181)" src="https://github.com/user-attachments/assets/82034334-7e60-4726-ace1-ea7af64b0cf0" />

### Verify Registration

After creation:

- Go back to **Target Groups**
- Select your newly created target group
- Click on the **Targets** tab
- You should see the EC2 instances with a **healthy** status after a few seconds (if health checks pass)


<img width="1920" height="1080" alt="Screenshot (189)" src="https://github.com/user-attachments/assets/28d66988-ef97-4aaf-8669-eb446e87fcef" />

## ✅ Step 3: Creating an Application Load Balancer and Associating It with a Target Group

After launching EC2 instances and registering them in a **Target Group**, the next step is to set up a **Load Balancer** to distribute traffic among them.


### Navigate to Load Balancers

1. Go to the **AWS Management Console**
2. Open the **EC2 Dashboard**
3. From the left-hand menu, click **Load Balancers**
4. Click **Create Load Balancer**

### Select Load Balancer Type

Choose **Application Load Balancer (ALB)**:
- Suitable for HTTP/HTTPS traffic
- Operates at **Layer 7** (Application Layer)

Click **Create** under Application Load Balancer.

<img width="1920" height="1080" alt="Screenshot (183)" src="https://github.com/user-attachments/assets/f80464ae-d937-4374-a316-0e17dd2d7e3f" />

### Configure Basic Settings

Fill in the required fields:
- **Name**: `ALB-Testing`
- **Scheme**: `Internet-facing`
- **IP address type**: `IPv4`
- **Listener**: `HTTP` on port `80`


<img width="1920" height="1080" alt="Screenshot (184)" src="https://github.com/user-attachments/assets/c23e3dc5-5661-4e2c-83ce-10d55ee3a64e" />

Click **Next: Configure Security Settings**

<img width="1920" height="1080" alt="Screenshot (185)" src="https://github.com/user-attachments/assets/8aa82a03-20de-4521-afdb-4fe6463effc3" />

### Register Target Group

In this step, you’ll associate the previously created **Target Group** with the Load Balancer.

- **Target group name**: Select your existing target group (e.g., `TargetGroup-1`)
- **Protocol**: HTTP
- **Port**: 80

Click **Next: Register Targets**

<img width="1920" height="1080" alt="Screenshot (187)" src="https://github.com/user-attachments/assets/7f050a7a-c28f-4e99-935e-f161b31e50a0" />

### Review and Create

1. Review all the configurations
2. Click **Create Load Balancer**

After a few moments, your load balancer will be **provisioned and active**.


### Access Your Load Balancer

1. Go to **EC2 → Load Balancers**
2. Copy the **DNS name** of your ALB (e.g., `my-alb-123456789.us-east-1.elb.amazonaws.com`)
3. Paste it in your browser:


> - _**Note: You cannot access the instance through the ALB DNS name why?
because the SG of ALB only allows to communicate within itself to one thing you can do you can add below rule to ALB SG this will work.**_

| Type | Protocol | Port | Source    |
| ---- | -------- | ---- | --------- |
| HTTP | TCP      | 80   | 0.0.0.0/0 |

- _**Exposing ALB to Internet is ok, but our instance also exposed to internet and hackers can directly reach the instance by the public ip, so we need to update instances SG and allow trusted traffic through ALB.**_

| Type | Port | Source                                                  |
| ---- | ---- | ------------------------------------------------------- |
| HTTP | 80   | ALB's Security Group ID ✅ (Only allow traffic from ALB)|

>-  _**Important: If using custom VPC then you need to create a new Security group for the ALB and allow traffic from every where note that ALB should be in the public subnet and anywhere rule in SG makesure to attach the SG of ALB to the server instance for security point of view**_


<img width="1920" height="1080" alt="Screenshot (193)" src="https://github.com/user-attachments/assets/b45d45ad-5505-41ef-a68a-c02c1e4a1c48" />






