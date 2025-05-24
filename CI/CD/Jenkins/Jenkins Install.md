
# Learn How to Configure a Secure CI Pipeline on Ubuntu 22.04 LTS

## Table of Contents
1. [Prerequisites](#prerequisites)  
2. [Install Jenkins](#1-install-jenkins)  
    - [Install Java](#installation-of-java)  
    - [Install Jenkins](#install-jenkins)  
    - [Accessing Jenkins](#accessing-jenkins)  
    - [Install Required Plugins](#install-required-jenkins-plugins)
7. [References](#references)  

---

## Prerequisites
Ensure you meet these requirements before proceeding:  
- Operating System: **Ubuntu 22.04 LTS Server or higher**  
- A user account with `sudo` privileges  
- Basic understanding of Linux commands  

---

## 1. Install Jenkins  

### 🔹 Installation of Java  
Jenkins requires Java to run. Install OpenJDK 21:

```bash
# Update system package index
sudo apt update

# Install Java and required packages
sudo apt install fontconfig openjdk-21-jre

# Verify Java installation
java -version


```

### 🔹 Install Jenkins Package
Add Jenkins' official repository and install Jenkins:

```bash

## Add Jenkins GPG key
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian/jenkins.io-2023.key

# Add Jenkins repository
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/" | \
sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

# Update repositories
sudo apt-get update

# Install Jenkins
sudo apt-get install jenkins

```
Enable, start, and check the Jenkins service status:

```bash

# Enable Jenkins at boot
sudo systemctl enable jenkins

# Start Jenkins service
sudo systemctl start jenkins

# Check Jenkins status
sudo systemctl status jenkins


```

If the default port `(8080)` is in use, change it:

```bash
# Edit Jenkins service configuration
sudo systemctl edit jenkins --full


# Add the following lines to the opened configuration file:
# These lines change the default port used by Jenkins from 8080 to 8081 (or any custom port you prefer).
# ---------------------------------
[Service]
Environment="JENKINS_PORT=8081"
# ---------------------------------

# Restart Jenkins with new port
sudo systemctl restart jenkins


```

### 🔹 Accessing Jenkins
Visit Jenkins at `http://<your_server_ip>:8080` (or the custom port you specified). Use this command to retrieve the initial admin password:

```bash
# This command retrieves the initial admin password for Jenkins from the specified file.
# The password is required for the first-time setup of Jenkins.
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

```
### 🔹 Install required Jenkins plugins
- Pipeline Stage View
- Docker
- Generic Webhook Trigger
- OWASP Dependency-Check
- SonarQube Scanner
- HTML Publisher

### 📖 References
🔹 [Jenkins Official Documentation](https://www.jenkins.io/doc/)

🔹 [Ubuntu Package Management](https://documentation.ubuntu.com/server/)