// Script file for pipeline script in 'Step_4_Pipeline.txt'
// For proper visualization purpose

pipeline {
    agent any

    environment {
        PATH = "$PATH:/opt/apache-maven-3.9.15/bin"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/githubUser/Project-App.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("Sonar-Server-9.9") {     // 'sonarqube-9.9.0.65466' is the extracted folder of SonarQube in instance
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Deploy to AWS EC2') {
            steps {
                sshagent(['ec2-tomcat-key']) {
                    sh 'scp -o StrictHostKeyChecking=no target/*.war <server_username>@<public_ip>:/home/ubuntu/apache-tomcat-9.0.80/webapps'
		            // Example: <target_file> <server_username>@<public_ip>:<deploy_folder_path> --> target/*.war ubuntu@43.205.236.128:/home/ubuntu/apache-tomcat-9.0.80/webapps
                }
            }
        }
    }
}
