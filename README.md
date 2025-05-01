# 🚀 CI/CD DevOps Pipeline Project

This project demonstrates a complete CI/CD pipeline for a Java Spring application using modern DevOps tools. The project is based on a forked Spring Boot application, to which I have added:

- A custom `Dockerfile`
- A `Jenkinsfile` for pipeline automation
- A Kubernetes `deployment.yaml` for container orchestration

---

## 📌 Project Overview

The goal is to automate the build, test, and deployment process for a Java application using Jenkins, SonarQube, Docker, and Kubernetes. The pipeline includes email notifications and integrates with GitHub and Docker Hub.

> You can use any Java application. In this project, I forked a Spring Boot Java application from GitHub:  
> `https://github.com/spring-projects/spring-petclinic.git`  
> (You can keep your fork URL to preserve the original link structure.)

---

## 🧰 Tools & Technologies

| Tool           | Purpose                              |
|----------------|--------------------------------------|
| CentOS Stream 9 | Base operating system                |
| Git & GitHub   | Source code version control          |
| Jenkins        | Pipeline orchestration               |
| Maven          | Build automation for Java            |
| SonarQube      | Static code analysis                 |
| Docker         | Containerization                     |
| Docker Hub     | Image repository                     |
| Kubernetes (Minikube) | Container orchestration        |
| Gmail SMTP     | Email notifications                  |

---

## 🗂️ Project Structure

├── Dockerfile ├── Jenkinsfile ├── pom.xml ├── README.md ├── src/ └── k8s/ └── deployment.yaml

## 🔄 CI/CD Pipeline Workflow

1. ✅ **Code Checkout** – Pull source from GitHub
2. 🧪 **Static Code Analysis** – Run SonarQube scan
3. ⚙️ **Build Application** – Use Maven to compile and package
4. 🐳 **Dockerize App** – Build and push Docker image to Docker Hub
5. ☸️ **Deploy to Kubernetes** – Apply deployment.yaml via `kubectl`
6. 📧 **Send Email Notifications** – Success or failure report via Gmail SMTP

---

## ⚙️ Setup & Installation Guide

### 🖥️ 1. Environment Setup

- **Base OS:** CentOS Stream 9 (can also use AWS EC2, Ubuntu, etc.)
- **Virtualization:** VMware Workstation, VirtualBox, or Cloud (e.g., AWS EC2)

### 📦 2. Install Dependencies

#### Git
```bash
sudo dnf install git -y
git --version

#### Maven
```bash
sudo dnf install maven -y
mvn -version
```

#### Docker
```bash
sudo dnf install docker -y
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
```

#### SonarQube (via Docker)
```bash
docker network create sonarnet
docker run -d --name sonarqube \
  -p 9000:9000 \
  --network sonarnet \
  sonarqube:latest
```
- Access at: http://localhost:9000
- Default login: admin / admin
- Generate a token: My Account > Security > Generate Token

#### Jenkins
```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io-2023.key
sudo dnf install jenkins -y
sudo systemctl enable --now jenkins
```
- Jenkins runs at: http://localhost:8080
- Admin password: /var/lib/jenkins/secrets/initialAdminPassword

  ## 🔌 Jenkins Setup

### ✅ Install Required Plugins

- Git Plugin  
- Maven Integration  
- SonarQube Scanner  
- Docker Pipeline  
- Kubernetes CLI Plugin  
- Email Extension Plugin  

---

### 🔐 Add Credentials (Manage Jenkins > Credentials > Global)

| Service      | Kind                  | ID (suggested)       |
|--------------|-----------------------|----------------------|
| **Docker Hub**  | Username + Password     | `dockerhub-creds`    |
| **SonarQube**   | Secret Text (token)     | `sonarqube-token`    |
| **GitHub**      | Username + Password     | `github-credentials` |
| **Gmail SMTP**  | Username + App Password | `jenkins-gmail`      |

---

### 🔧 SonarQube Integration

- Manage Jenkins > Configure System  
- Scroll to **SonarQube servers**
  - Name: `sonarqube`
  - Server URL: `http://localhost:9000`
  - Token: Use the token credential ID

---

### 📩 Gmail SMTP Integration

- Manage Jenkins > Configure System  
- SMTP Server: `smtp.gmail.com`  
- Port: `587`  
- Use TLS: ✅  
- Username: your Gmail address  
- Password: Gmail App Password  
- Default Recipients: your email

---

## 🚀 How to Run the Pipeline

1. Fork and clone the project:
    ```bash
    git clone https://github.com/your-username/your-forked-repo.git
    cd your-forked-repo
    ```

2. Commit your `Dockerfile`, `Jenkinsfile`, and `deployment.yaml` if not already pushed.

3. Run the Jenkins pipeline from the Jenkins dashboard.

4. Monitor:
   - Build logs  
   - Docker Hub image  
   - SonarQube quality report  
   - Minikube dashboard  
   - Email inbox for notification
