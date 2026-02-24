# Hosting a Static Website on AWS EC2
# Clone This Repository
```bash
git clone https://github.com/atulkamble/aws-ec2-static-website.git
cd aws-ec2-static-website
```

# AWSWebsiteProject
**Hosting a Website on AWS EC2**

This project provides a comprehensive guide for configuring and hosting a static website on AWS EC2, including relevant code snippets for setup and deployment.

---

### **Project: Hosting a Static Website on AWS EC2**

**Objective:**  
Establish and deploy a static website on an AWS EC2 instance.

---

### **Prerequisites**

1. **AWS Account:** Ensure you have an active AWS account.
2. **AWS CLI:** Install and configure the AWS Command Line Interface (CLI). [Download AWS CLI](https://aws.amazon.com/cli/)
3. **EC2 Key Pair:** Generate an EC2 key pair for secure instance access.
4. **Git:**
   ```bash
   git config --global user.name "username"
   git config --global user.email "email@example.com"
   ```

---

### **Step 1: Launch an EC2 Instance**

1. **Access AWS Management Console** and navigate to the EC2 Dashboard.

2. **Initiate Instance Launch:**
   - Select “Launch Instance”.
   - Choose an Amazon Machine Image (AMI): Opt for the “Amazon Linux 2 AMI”.
   - Select an Instance Type: Choose “t2.micro” (eligible for the free tier).
   - Configure Instance: Accept the default configuration settings.
   - Add Storage: Accept the default storage configuration.
   - Add Tags: (Optional) Add a tag such as `Name: EC2WebServer`.
   - Configure Security Group: Create a new security group with rules allowing HTTP (port 80) and SSH (port 22) traffic.
   - Review and Launch: Verify settings and click “Launch”. Select your key pair and proceed with launching the instance.

---

### **Step 2: Connect to Your EC2 Instance**

1. **Open your terminal** (Linux/Mac) or use PuTTY (Windows).

2. **Establish SSH Connection:**
   ```bash
   ssh -i /path/to/your-key-pair.pem ec2-user@your-ec2-public-dns
   ```

---

### **Step 3: Install and Configure Apache Web Server**

1. **Update the instance and install Apache HTTP Server:**
   ```bash
   sudo yum update -y
   sudo yum install -y httpd
   ```

2. **Start Apache service and enable it to start on boot:**
   ```bash
   sudo systemctl start httpd
   sudo systemctl enable httpd
   sudo usermod -a -G apache ec2-user
   ```

---

### **Step 4: Deploy Your Static Website**

1. **Create a basic HTML file:**
   ```bash
   sudo tee /var/www/html/index.html <<EOF
   <html>
   <head>
       <title>My First AWS Website</title>
   </head>
   <body>
       <h1>Hello, World!</h1>
       <p>Welcome to my website hosted on AWS EC2!</p>
   </body>
   </html>
   EOF
   ```

2. **Set file permissions (if required):**
   ```bash
   sudo chmod 644 /var/www/html/index.html
   ```

3. **Verify deployment by accessing the instance’s public DNS in your browser:**
   ```
   http://your-ec2-public-dns
   ```

   You should see the “Hello, World!” webpage.

---

### **Step 5: (Optional) Automate Deployment with a Script**

1. **Create a deployment script (`deploy_website.sh`):**
   ```bash
   #!/bin/bash
   sudo yum update -y
   sudo yum install -y httpd
   sudo systemctl start httpd
   sudo systemctl enable httpd
   sudo usermod -a -G apache ec2-user

   sudo tee /var/www/html/index.html <<EOF
   <html>
   <head>
       <title>My First AWS Website</title>
   </head>
   <body>
       <h1>Hello, World!</h1>
       <p>Welcome to my website hosted on AWS EC2!</p>
   </body>
   </html>
   EOF

   sudo chmod 644 /var/www/html/index.html
   ```

2. **Transfer and execute the script on your EC2 instance:**
   ```bash
   scp -i /path/to/your-key-pair.pem deploy_website.sh ec2-user@your-ec2-public-dns:/home/ec2-user/
   ssh -i /path/to/your-key-pair.pem ec2-user@your-ec2-public-dns 'bash /home/ec2-user/deploy_website.sh'
   ```

---

### **Conclusion**

You have successfully set up and hosted a static website on an AWS EC2 instance. This guide walks you through launching an EC2 instance, installing a web server, and deploying a basic HTML page. For more advanced setups, consider integrating additional AWS services or using infrastructure-as-code tools such as Terraform or Ansible.

---
