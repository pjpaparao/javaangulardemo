pipeline {
    agent any

    tools {
        maven 'Maven_3.9'
        jdk 'JDK_17'
    }

    stages {
        stage('Build') {
            steps {
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
                bat 'docker build -t javaangulardemo-app .'
                bat 'docker rm -f javaangulardemo-container || exit 0'
                bat 'docker run -d --name javaangulardemo-container -p 8081:8080 javaangulardemo-app'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed! Check console output for logs.'
        }
    }
}
