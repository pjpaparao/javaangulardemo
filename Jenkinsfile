pipeline {
    agent any

    tools {
        jdk 'JDK_17'
        maven 'Maven_3.9'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'develop', url: 'https://github.com/pjpaparao/javaangulardemo.git'
            }
        }

        stage('Build') {
            steps {
                // Run Maven at repo root (where pom.xml exists)
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                withEnv(['DOCKER_HOST=tcp://localhost:2375']) {
                    bat 'docker build -t javaangulardemo-app .'
                    bat 'docker rm -f javaangulardemo-container >nul 2>&1 || ver >nul'
                    bat 'docker run -d --name javaangulardemo-container -p 8081:8080 javaangulardemo-app'
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
