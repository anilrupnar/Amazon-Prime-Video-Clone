# Amazon Video Clone Deployment Automation

## Objective
Automate the deployment of an Amazon Video clone application using Docker and Jenkins to streamline development and deployment processes, ensuring efficient management of the application lifecycle.

---
## Prerequisites

1. **AWS Account**: Access to AWS EC2 for hosting the application.
2. **Docker**: Installed on your local machine or EC2 instance.
3. **Jenkins**: Installed and configured for CI/CD.
4. **SonarQube**: Configured for code quality checks.
5. **Trivy**: Installed for Docker image vulnerability scanning.
6. **Git Repository**: Version-controlled source code for the application.

---

## Key Features

1. **Automated Deployment Pipeline**:
   - CI/CD pipeline automates the build, test, and deployment processes for the application.

2. **Containerization with Docker**:
   - Consistent containerized environments across development, testing, and production.

3. **Code Quality Checks**:
   - Integrated **SonarQube** to analyze source code for bugs, vulnerabilities, and code smells.

4. **Security Scanning**:
   - **Trivy** scans Docker images for vulnerabilities to enhance application security.

5. **Scalable Infrastructure**:
   - Deployed on AWS EC2, allowing scalable resource management based on application demands.

6. **User-Friendly Interface**:
   - Jenkins provides a web-based interface for pipeline management and monitoring.

7. **Customizable Configuration**:
   - Flexible pipeline to include integration tests, performance tests, or notifications.

8. **Version Control Integration**:
   - Integration with Git for automatic builds and deployments triggered by code changes.

9. **Environment Consistency**:
   - Docker ensures uniform application behavior across environments.

10. **Documentation and Instructions**:
    - Clear setup instructions to replicate the deployment process easily.

11. **Real-Time Monitoring and Feedback**:
    - Monitor deployment in real-time via Jenkins logs and feedback.

---

## Technologies Used

| Technology         | Purpose                                                                 |
|---------------------|-------------------------------------------------------------------------|
| **Amazon EC2**      | Hosting the application on a scalable virtual server.                  |
| **Docker**          | Containerization for consistent application environments.              |
| **Jenkins**         | Automating CI/CD processes.                                            |
| **SonarQube**       | Code quality management for detecting vulnerabilities and code smells. |
| **Trivy**           | Vulnerability scanning for Docker images.                             |
| **Git**             | Version control for source code management.                           |
| **Linux (Ubuntu)**  | OS for hosting the tools and application.                              |
| **SSH**             | Secure remote access to EC2 instances.                                |
| **HTTP/HTTPS**      | Secure communication for web access.                                   |
| **Pipeline as Code**| Manage Jenkins pipelines with code for version control and automation. |

---

## Setup Instructions

# Launch an EC2 Instance on AWS

## Overview
This guide provides step-by-step instructions to launch an Amazon EC2 instance configured for development or deployment purposes.

## Steps to Launch an EC2 Instance

### 1. Sign in to AWS
- Log in to the [AWS Management Console](https://aws.amazon.com/console/).
- Navigate to **Services > EC2** to access the EC2 Dashboard.

#### 2. Launch an Instance
1. **Name and Tags**  
   - Provide a name for your instance (e.g., `prime-eks`).

2. **Choose an AMI**  
   - Select **Ubuntu 24.04 LTS** (free-tier eligible).

3. **Select Instance Type**  
   - Choose **t2.medium** for sufficient CPU and memory resources.

#### 3. Configure Instance Details
- Leave the default settings unless customization is required.
- Ensure **Auto-assign Public IP** is **enabled** for remote access.

#### 4. Add Storage
- Allocate **25 GiB** or more, depending on the requirements of your application.

#### 5. Configure Security Group
Create a new security group and add the following inbound rules:

| Type         | Protocol | Port Range | Source              | Purpose                                   |
|--------------|----------|------------|---------------------|-------------------------------------------|
| SSH          | TCP      | 22         | My IP               | For secure remote access using SSH.      |
| HTTP         | TCP      | 80         | Anywhere (0.0.0.0/0)| Allow web traffic.                       |
| HTTPS        | TCP      | 443        | Anywhere (0.0.0.0/0)| Allow secure web traffic (SSL/TLS).      |
| Custom TCP   | TCP      | 9000       | Anywhere (0.0.0.0/0)| For SonarQube or similar services.       |
| Custom TCP   | TCP      | 8080       | Anywhere (0.0.0.0/0)| For Jenkins or other tools.              |

#### 6. Review and Launch
- Confirm all configuration settings and click **Launch**.

#### 7. Key Pair
- Create a new key pair or use an existing one for SSH access:
  - Download the `.pem` file securely.

### 2. Connect to the Instance Using MobaXterm and `.pem` Key File

To connect to the EC2 instance using MobaXterm and the `.pem` key file, follow these steps:

1. **Open MobaXterm**: Launch MobaXterm on your local device.
2. **Start a New SSH Session**:
   - Click **Session** > **SSH**.
   - Enter the **Public IPv4 address** of your EC2 instance in the **Remote host** field.
   - Use `ubuntu` as the **username** (for Ubuntu instances).
3. **Add the `.pem` Key for Authentication**:
   - Check **Use private key**.
   - Click **Browse** to locate your `.pem` file (e.g., `project_access_key.pem`).
4. **Connect to the Instance**:
   - Click **OK** to start the connection.
   - If prompted with a security warning, confirm to proceed.
5. **Successful Connection**:
   - You should now have access to your EC2 instance's terminal.

### Step 3: Install Jenkins

1. **Download and Run Jenkins**:
   
```bash
#!/bin/bash
   sudo apt update -y
   wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo tee /etc/apt/keyrings/adoptium.asc
   echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
   sudo apt update -y
   sudo apt install temurin-17-jdk -y
   /usr/bin/java --version
   curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
   echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
   sudo apt-get update -y
   sudo apt-get install jenkins -y
   sudo systemctl start jenkins
   sudo systemctl status jenkins

```

2. **Access Jenkins**:
   - Open Jenkins in your browser by copying the public IP address of the EC2 instance and pasting it into the address bar of your browser, followed by `:8080`. 
   - Example: `publicIP:8080`
   
   If you are unable to see the Jenkins login page, check whether Jenkins is running in the terminal.
   
   After entering the IP address followed by `:8080` in the browser, the login page will appear, prompting for a password.
   
 ![login page ](https://github.com/anilrupnar/Deploying-Virtual-Browser/blob/main/images/login%20page.png )
   
To retrieve the password:
   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```
- Access the terminal where your EC2 instance is open and run the command to display the initial admin password needed to proceed with the Jenkins setup.
- Copy the code and paste it into the Jenkins Getting Started screen.
- Follow the on-screen instructions to complete the setup. Once done, you’ll be directed to the Jenkins dashboard.
