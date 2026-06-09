# 🚀 CI/CD Pipeline: Jenkins • SonarQube • Tomcat

![CI/CD](https://img.shields.io/badge/CI%2FCD-Automation-blue)
![Jenkins](https://img.shields.io/badge/Jenkins-Used-red)
![SonarQube](https://img.shields.io/badge/SonarQube-Code%20Quality-green)
![Tomcat](https://img.shields.io/badge/Tomcat-Deployment-orange)
![Maven](https://img.shields.io/badge/Maven-Build-yellow)


## 📌 Overview

End-to-end **CI/CD pipeline project** that automates build, code analysis, and deployment of a Java application using Jenkins, SonarQube, and Apache Tomcat.


## ☁️ Infrastructure (AWS)

All services are deployed on AWS EC2 instances:

- Jenkins → EC2 instance (CI server)
- SonarQube → EC2 instance (Code Quality server)
- Tomcat → EC2 instance (Application server)

Security groups were configured to allow required ports between services.


## ⚙️ Tech Stack

- AWS (EC2) – Infrastructure / Servers
- Jenkins – CI/CD Orchestration  
- SonarQube – Code Quality Analysis  
- Apache Tomcat – Application Deployment  
- Maven – Build Automation  
- Git & GitHub – Version Control  
- Java – Application  


## 🏗️ Architecture

Developer → GitHub → AWS EC2 (Jenkins) → AWS EC2 (SonarQube) → AWS EC2 (Tomcat)


## 🔄 Pipeline Flow

- Code pushed to GitHub  
- Jenkins triggers pipeline  
- Maven builds application  
- SonarQube analyzes code quality  
- WAR artifact generated  
- Deployed automatically to Tomcat  
- Application goes live  


## 📁 Project Structure

```
jenkins-sonarqube-tomcat-cicd/
├── docs/
├── screenshots/
├── utilities/
├── Jenkinsfile
└── README.md
```


## ✨ Highlights

- Fully automated CI/CD workflow  
- Integrated static code analysis (SonarQube)  
- Automated deployment to Tomcat  
- Modular documentation for easy understanding  


## 📸 Screenshots

_Add screenshots here (Jenkins pipeline, SonarQube report, Tomcat deployment)_  


## 🚀 Future Enhancements

- Dockerized CI/CD environment  
- Kubernetes deployment  
- AWS cloud integration  
- Slack / Email notifications  
- Nexus artifact repository  


## 👤 Author

**Vishal Bhardwaj**  
GitHub: https://github.com/vishal2github
