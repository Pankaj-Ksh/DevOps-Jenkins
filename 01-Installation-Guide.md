# 🚀 Jenkins Installation on Amazon Linux 2023 (EC2)

This guide explains how to install and configure **Jenkins** on an **Amazon Linux 2023 EC2 instance** using `dnf`.
### 🔑 Key Points about `dnf`

- It is used to **install, update, remove, and manage software packages**.  
- Provides **better performance** than `yum`.  
- Handles **dependencies automatically** (installs all required packages for a program).  
- Supports modern features like **parallel downloading**.  
- On **Amazon Linux 2023**, `yum` is replaced by `dnf`.  

---

## ✅ Prerequisites

- **AWS EC2 Instance**  
  - Must be running **Amazon Linux 2023**  
  - Recommended size: **t2.medium** or higher  

- **Security Group**  
  - Allow inbound traffic on:  
    - **Port 22** → for SSH access  
    - **Port 8080** → for Jenkins Web UI  

- **Permissions**  
  - You need `sudo` access to run installation commands  

---

# ⚙️ Installation Steps and Commands 

## 0. Switch to root user 
```bash 
sudo su
```

## 1. Add the Jenkins repository
```bash 
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

## 2. Import the Jenkins GPG key to verify packages
```bash
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```

## 3. Update your system's package list
```bash 
dnf upgrade -y
```

## 4. Install Java 17 (Amazon Corretto) and fontconfig, which are required dependencies for Jenkins
```bash 
dnf install -y fontconfig java-17-amazon-corretto
```

## 5. Install Jenkins itself
```bash
dnf install -y jenkins
```

## 6. Reload the systemd manager configuration to recognize the new Jenkins service
```bash
systemctl daemon-reload
```

## 7. Enable Jenkins to start automatically on boot
```bash
systemctl enable jenkins
```

## 8. Start the Jenkins service
```bash
systemctl start jenkins
```

## 9. Verify that the Jenkins service is running correctly
```bash
systemctl status jenkins
```

## 10. Access Jenkins Open your web browser and navigate to:
    http://<Your-EC2-Public-IP>:8080

## 11. Display the initial administrator password
``` bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

## Ready to use Script :
```
dnf install -y git maven

wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

dnf upgrade -y

dnf install -y fontconfig java-17-amazon-corretto

dnf install -y jenkins

systemctl daemon-reload

systemctl enable jenkins

systemctl start jenkins

systemctl status jenkins
```
