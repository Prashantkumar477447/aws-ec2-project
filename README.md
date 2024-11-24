# aws-ec2-project 
# Deploy a Simple Web Application on AWS EC2

This project demonstrates how to deploy a simple web application (such as Jenkins) on an AWS EC2 instance and make it accessible from outside AWS. It includes steps for provisioning resources, configuring the server, and accessing the application via a public URL.

---

## **Features**
- Deployment of Jenkins, a popular CI/CD tool, on an EC2 instance.
- Secure and scalable infrastructure setup.
- Accessing the application through a public domain or IP.
- Detailed configuration instructions.

---

## **Prerequisites**
1. **AWS Account**: Ensure you have an active AWS account.
2. **AWS CLI**: Installed and configured on your local machine.
3. **EC2 Key Pair**: Create a key pair in AWS to securely access the EC2 instance.
4. **Security Group**: Configure security group rules to allow HTTP and SSH traffic.
5. **Basic Knowledge of Jenkins**: Familiarity with its functionality is helpful but not mandatory.

---

## **Setup Instructions**

### **1. Launch an EC2 Instance**
1. Log in to your AWS Management Console.
2. Navigate to **EC2** and click **Launch Instances**.
3. Choose an Amazon Machine Image (AMI), e.g., Ubuntu 22.04 LTS.
4. Select an instance type (e.g., t2.micro for free tier).
5. Configure the following:
   - Key Pair: Select an existing one or create a new key pair.
   - Security Group: 
     - Allow **HTTP (port 80)** and **SSH (port 22)**.
     - Allow the application's port (e.g., **8080** for Jenkins).
6. Launch the instance.
![Screenshot 2024-11-06 203912](https://github.com/user-attachments/assets/177b56b8-1fab-489c-ba00-67524e9623f0)
---

### **2. SSH into the EC2 Instance**
Obtain the public IP or DNS of your EC2 instance from the AWS Console. Use the following command to connect:
```bash
ssh -i <your-key.pem> ubuntu@<your-ec2-public-ip>
```
![ssh](https://github.com/user-attachments/assets/6aaf22c0-59fb-4998-a79d-6757a7d81265)

---

### **3. Install Jenkins**
1. Update the package manager:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. Install Java (Jenkins requires Java 17 or higher):
   ```bash
   sudo apt install openjdk-17-jdk -y
   ```
3. Add the Jenkins repository:
   ```bash
   curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
       /usr/share/keyrings/jenkins-keyring.asc > /dev/null
   echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
       https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
       /etc/apt/sources.list.d/jenkins.list > /dev/null
   sudo apt update
   ```
4. Install Jenkins:
   ```bash
   sudo apt install jenkins -y
   ```
5. Start and enable the Jenkins service:
   ```bash
   sudo systemctl start jenkins
   sudo systemctl enable jenkins
   ```
![Screenshot 2024-11-24 215259](https://github.com/user-attachments/assets/416d4890-7ddd-4c49-8407-44118b7ee748)

---

### **4. Configure Security Group for Jenkins**
Allow inbound traffic to Jenkins' default port (8080):
1. Go to **EC2 > Security Groups** in the AWS Console.
2. Edit the inbound rules and add:
   - Protocol: TCP
   - Port: 8080
   - Source: 0.0.0.0/0 (For public access).

---

### **5. Access Jenkins from Outside AWS**
1. Open your web browser.
2. Navigate to:
   ```
   http://<your-ec2-public-ip>:8080
   ```
3. Unlock Jenkins:
   - Retrieve the initial admin password:
     ```bash
     sudo cat /var/lib/jenkins/secrets/initialAdminPassword
     ```
   - Enter it in the browser and follow the setup wizard.
![Screenshot 2024-11-24 214751](https://github.com/user-attachments/assets/bbc80373-c5c1-444b-8960-15c76e8e45fc)
![Screenshot 2024-11-24 214818](https://github.com/user-attachments/assets/209bf0ef-360b-40fa-a7ae-56322b8b16e0)


---

## **Additional Notes**
- **Security**: Avoid using `0.0.0.0/0` in production environments. Restrict access to trusted IP ranges.
- **Scaling**: For scalability, consider using AWS Elastic Load Balancer (ELB) and Auto Scaling Groups.
- **Persistence**: Use Amazon EBS for storing application data persistently.

---

## **Project Structure**
```plaintext
├── README.md      # Project documentation
├── scripts/       # Bash scripts for automation (optional)
└── diagrams/      # Architecture diagrams (optional)
```

---

## **Future Improvements**
- Automate deployment using Infrastructure-as-Code (IaC) tools like Terraform.
- Use AWS Application Load Balancer (ALB) for high availability.
- Secure application with HTTPS using Let's Encrypt.

---

## **Contributing**
Contributions are welcome! Please fork the repository, make your changes, and submit a pull request.

---

## **License**
This project is licensed under the MIT License. See the LICENSE file for details.

---

### **Contact**
For any issues or suggestions, feel free to create an issue in this repository or contact me directly.
