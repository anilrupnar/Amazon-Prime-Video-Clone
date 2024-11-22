# Amazon Video Clone Deployment Automation

## Objective
Automate the deployment of an Amazon Video clone application using Docker and Jenkins to streamline development and deployment processes, ensuring efficient management of the application lifecycle.

## Project Architecture Diagram

   ![Project Architecture Diagram ](https://github.com/anilrupnar/Deploying-Virtual-Browser/blob/main/images/Project%20Architecture%20Diagram.gif)

## Prerequisites

1. **AWS Account**: Access to AWS EC2 for hosting the application.
2. **Docker**: Installed on your local machine or EC2 instance.
3. **Jenkins**: Installed and configured for CI/CD.
4. **SonarQube**: Configured for code quality checks.
5. **Trivy**: Installed for Docker image vulnerability scanning.
6. **Git Repository**: Version-controlled source code for the application.


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


## Technologies Used

| Technology         | Purpose                                                                 |
|---------------------|-------------------------------------------------------------------------|
| **Amazon EC2**      | Hosting the application on a scalable virtual server.                  |
| **Docker**          | Containerization for consistent application environments.              |
| **Jenkins**         | Automating CI/CD processes.                                            |
| **SonarQube**       | Code quality management for detecting vulnerabilities and code smells. |
| **Trivy**           | Vulnerability scanning for Docker images.                             |
| **Git**             | Version control for source code management.                           |


# Setup Instructions

## Steps to Launch an EC2 Instance

### 1. Sign in to AWS
- Log in to the [AWS Management Console](https://aws.amazon.com/console/).
- Navigate to **Services > EC2** to access the EC2 Dashboard.

### 2. Launch an Instance
1. **Name and Tags**  
   - Provide a name for your instance (e.g., `prime-eks`).

2. **Choose an AMI**  
   - Select **Ubuntu 24.04 LTS** (free-tier eligible).

3. **Select Instance Type**  
   - Choose **t2.medium** for sufficient CPU and memory resources.

### 3. Configure Instance Details
- Leave the default settings unless customization is required.
- Ensure **Auto-assign Public IP** is **enabled** for remote access.

### 4. Add Storage
- Allocate **25 GiB** or more, depending on the requirements of your application.

### 5. Configure Security Group
Create a new security group and add the following inbound rules:

| Type         | Protocol | Port Range | Source              | Purpose                                   |
|--------------|----------|------------|---------------------|-------------------------------------------|
| SSH          | TCP      | 22         | My IP               | For secure remote access using SSH.      |
| HTTP         | TCP      | 80         | Anywhere (0.0.0.0/0)| Allow web traffic.                       |
| HTTPS        | TCP      | 443        | Anywhere (0.0.0.0/0)| Allow secure web traffic (SSL/TLS).      |
| Custom TCP   | TCP      | 9000       | Anywhere (0.0.0.0/0)| For SonarQube or similar services.       |
| Custom TCP   | TCP      | 8080       | Anywhere (0.0.0.0/0)| For Jenkins or other tools.              |

### 6. Review and Launch
- Confirm all configuration settings and click **Launch**.

### 7. Key Pair
- Create a new key pair or use an existing one for SSH access:
  - Download the `.pem` file securely.

## Step 2. Connect to the Instance Using MobaXterm and `.pem` Key File

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

## Step 3: Install Jenkins

### 1. Download and Run Jenkins:
   
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
  ![Jenkins status ](https://github.com/anilrupnar/Deploying-Virtual-Browser/blob/main/images/Project%20Architecture%20Diagram.gif)
  
### 2. Access Jenkins:
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

### 3. Then click on Installed Suggested Plugins and then this appears:

 ![Suggested Plugins ](https://github.com/anilrupnar/Deploying-Virtual-Browser/blob/main/images/login%20page.png )


## Step 4: Install Docker and Set Up SonarQube

Since Docker isn’t installed on our EC2 instance yet, let’s install Docker and Docker Compose, then set up SonarQube for code analysis.

### 1. Install Docker:
   - Run the command to install Docker:
     ```bash
     sudo apt install docker.io  # Install Docker
     ```


### 2. Adjust Docker Permissions:
   - After installing Docker Compose, execute the following command to adjust permissions for other users to access the Docker socket:
     ```bash
     sudo chmod 666 /var/run/docker.sock  # Grant access to Docker socket
     ```

### 3. Install SonarQube:
   - Now, set up SonarQube on our EC2 instance using Docker with the following command:
     ```bash
     docker run -d -p 9000:9000 sonarqube:lts-community  # Run SonarQube container
     ```
   - This command starts a SonarQube server in detached mode, mapping port 9000 on the host to port 9000 on the container. This setup enables us to perform code analysis and quality checks.
     
 

### 4. Access SonarQube and Configure SonarQube
Access the SonarQube web interface using a browser:  
- **Local Access**: `http://localhost:9000`  
- **Remote Access**: Replace `localhost` with your EC2 instance's public IP address. Example:  
  `http://<publicIP>:9000`  

SonarQube runs on port `9000` and allows you to analyze your project's code quality.

  ![SonarQube server login ]( )
  
### 5. Initial Login
Default **username**: `admin`  
Default **password**: `admin`  
After the first login, you will be prompted to change the password for security.

 ![Intial username ]( )
### 6. Create a SonarQube Token
1. Navigate to **Administration → Security → Users → Tokens**.
2. Enter a **Token Name**: `sonar`.
3. Click **Generate** and copy the **Sonar-Secret Token**.
4. Save this token securely, as it will not be displayed again.

  ![SonarQube server login ]( )
  
### 7. Set Up a Project in SonarQube
1. Go to **Projects → Manually**.
2. Enter the following details:
   - **Project Display Name**: `prime-clone`
   - **Project Key**: `prime-clone`
   - **Main Branch Name**: `main`
3. Click **Set Up → Locally**.
4. Choose **Use Existing Value** and paste the **Sonar-Secret Token**.
5. Select **Continue → Other → Linux**.
6. Copy the provided commands to set up and run the SonarQube Scanner on your local machine.

![Setup Project 1 ]( )

![Setup Project 2 ]( )

![Setup Project 3 ]( )

![Setup Project 4 ]( )


### 8. Configure Quality Gates
1. Navigate to **Quality Gate → Create**.
2. Enter the following details:
   - **Name**: `Jenkins`
   - Click **Save**.

![Setup Quality Gate 1 ]( )

![Setup Quality Gate 2 ]( )

![Setup Quality Gate 3 ]( )

### 9. Configure a Webhook for Jenkins
1. Go to **Administration → Configurations → Webhooks**.
2. Click **Create** and provide the following details:
   - **Name**: `Jenkins`
   - **URL**: `<jenkins-url>/sonarqube-webhook/`  
     Example: `http://<publicIP>:8080/sonarqube-webhook/`
3. Click **Create**.

![Setup Project 5 ]( )

![Setup Project 6 ]( )

## Stage 5: Installing Trivy on EC2 

```bash
sudo apt-get install wget apt-transport-https gnupg lsb-release -y
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy -y
```
 ![login page ](https://github.com/anilrupnar/Deploying-Virtual-Browser/blob/main/images/login%20page.png )
 
## Step 6: Install Plugins and Configure Jenkins

### 1. Install Plugins
1. Navigate to **Manage Jenkins → Plugins → Available Plugins**.
2. Search and install the following plugins:
   - **NodeJS**
   - **Pipeline: Stage View**
   - **SonarQube Scanner**
   - **Docker**
   - **Docker Commons**
   - **Docker Pipeline**
   - **Docker API**
   - **Eclipse Temurin Installer**
3. Click **Install** to add these plugins to Jenkins.

![image](https://github.com/user-attachments/assets/aa1b8146-d915-42c1-9567-ba0bda6f908a)


### 2. Add Credentials for SonarQube and DockerHub
1. Go to **Manage Jenkins → Credentials → Global → Add Credentials**.
2. Add **SonarQube Token**:
   - **Kind**: Secret Text
   - **Secret**: Paste the secret token from SonarQube.
   - **ID**: `sonar-cred`
   - **Description**: `sonar-cred`
   - Click **Create**.

![jenkins steup]()

3. Add **DockerHub Credentials**:
   - **Kind**: Username with Password
   - **Username**: Your DockerHub username.
   - **Password**: Your DockerHub password.
   - **ID**: `docker-cred`
   - **Description**: `docker-cred`
   - Click **Create**.


### 3. Configure SonarQube in Jenkins
1. Go to **Manage Jenkins → System → SonarQube Installations**.
2. Click **Add SonarQube** and fill in the following details:
   - **Name**: `sonar`
   - **Server URL**: URL of your SonarQube server (e.g., `http://<publicIP>:9000`).
   - **Sonar Authentication Token**: Select `sonar-cred` from the dropdown.
3. Click **Apply** and **Save**.

![ Manage Jenkins ]( )


### 4. Configure Tools in Jenkins
1. Go to **Manage Jenkins → Global Tool Configuration**.
2. Configure **JDK**:
   - Scroll to **JDK Installations**.
   - Click **Add JDK**.
   - **Name**: `jdk17`
   - Check **Install Automatically**.

![ JDK Installations ]( )

4. Configure **SonarQube Scanner**:
   - Scroll to **SonarQube Scanner Installations**.
   - Click **Add SonarQube Scanner**.
   - **Name**: `sonar`
   - Check **Install Automatically**.
  
![ SonarQube Scanner Installlation  ]( )

5. Configure **NodeJS**:
   - Scroll to **NodeJS Installations**.
   - Click **Add NodeJS**.
   - **Name**: `node16`
   - **Version**: `NodeJS 16.20.0`
6. Click **Apply** and **Save**.

![ NodeJS ]( )

## Step 7: Implement the CI/CD Pipeline in Jenkins

### Create a Pipeline Job
1. Go to the Jenkins dashboard and click **New Item**.
2. Enter **Name**: `prime-CICD`.
3. Select **Pipeline** and click **OK**.
4. Configure the job:
   - **Discard old builds**: Check this option and set **Max # of builds to keep** to `2`.
   - **Pipeline Definition**: Choose **Pipeline script** and paste the pipeline code below.
5. Save the configuration.


![ prime-CICD ]( )


### Pipeline Code

```groovy
pipeline {
    agent any

    tools {
        jdk 'jdk17'
        nodejs 'node16' // Ensure 'node16' is configured in Jenkins under Manage Jenkins > Global Tool Configuration
    }
    environment {
        SCANNER_HOME = tool 'sonar'
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/harshitsahu2311/Amazon-Prime-Clone-App.git'
            }
        }

        stage("SonarQube Analysis") {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '''
                        $SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.projectName=prime-clone \
                        -Dsonar.projectKey=prime-clone
                    '''
                }
            }
        }

        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-cred'
                }
            }
        }

        stage("Trivy File Scan") {
            steps {
                sh "trivy fs . > trivy.txt"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image tagged as 'prime-clone:latest'
                    sh "docker build -t prime-clone:latest ."
                }
            }
        }

        stage("Trivy Image Scan") {
            steps {
                sh "trivy image prime-clone:latest"
            }
        }

        stage('Tag & Push to DockerHub') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred') {
                        sh "docker tag prime-clone:latest harshitsahu2311/prime-clone:latest"
                        sh "docker push harshitsahu2311/prime-clone:latest"
                    }
                }
            }
        }

        stage("Deploy to Container") {
            steps {
                sh 'docker run -d --name amazon-prime -p 80:80 harshitsahu2311/prime-clone:latest'
            }
        }
    }
}
```

## Step 8: Build  the Pipeline

## Step 8: Run the Pipeline
- After saving the pipeline configuration, go to the pipeline job (`prime-CICD`) in Jenkins.
- Click **Build Now** to trigger the CI/CD pipeline.

![ build console ]( )

![ build successful ]( )

## Step 9: Final Application Deployed Successfully

![ final output ]( )

## Application Deployed Successfully on DockerHub

![ dockerhub status ]( )

## Deployed Pipeline Console Snapshot

![ pipeline console ]( )

## Deployed Pipeline Stages Snapshot

![ pipeline stages ]( )


**Thank you for reading my README file! 😊**

**Feel free to connect with me:**

- **LinkedIn**: [Anil Rupnar](https://www.linkedin.com/in/anilrupnar/)
- **Email**: anilrupnar2003@gmail.com


**Project Creadit**
 - Miroslav Šedivý : https://github.com/m1k1o
