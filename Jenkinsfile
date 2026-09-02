pipeline {
    agent any

    tools {
        jdk 'JDK_17'
        maven 'Maven_3.9'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from develop branch
                git branch: 'develop', url: 'https://github.com/pjpaparao/springboot.git'
            }
        }

        stage('Build') {
            steps {
                dir('jenkinpipelnedemo') {
                    // Compile and package application
                    bat 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Test') {
            steps {
                dir('jenkinpipelnedemo') {
                    // Run unit tests
                    bat 'mvn test'
                }
            }
        }

        stage('Deploy') {
            steps {
                dir('jenkinpipelnedemo') {
                    withEnv(['DOCKER_HOST=tcp://localhost:2375']) {
                        // 1. Build application Docker image
                        bat 'docker build -t springboot-app .'
                        
                        // 2. Stop and remove old container if it exists
                        bat 'docker rm -f springboot-app-container >nul 2>&1 || ver >nul'
                        
                        // 3. Start Spring Boot container directly
                        bat 'docker run -d --name springboot-app-container -p 8080:8080 springboot-app'
                    }
                }
            }
        }
    }

    post {
        success { 
            echo 'Pipeline executed successfully! App is available at http://localhost:8080/' 
        }
        failure { 
            echo 'Pipeline failed! Check console output for logs.' 
        }
    }
}