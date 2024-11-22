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

### 1. Clone the Repository
```bash
git clone https://github.com/your-repo/amazon-video-clone.git
cd amazon-video-clone
