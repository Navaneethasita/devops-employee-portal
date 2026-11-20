
pipeline {
    agent any

    tools {
        jdk 'jdk21'
        maven 'maven3'
    }

    environment {
        IMAGE_NAME = 'employee-portal'
        CONTAINER_NAME = 'employee-portal'
        HOST_PORT = '8082'
        APP_PORT = '8081'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Navaneethasita/devops-employee-portal.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t employee-portal:v${BUILD_NUMBER} .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f employee-portal || true

                    docker run -d \
                      --name employee-portal \
                      -p 8082:8081 \
                      employee-portal:v${BUILD_NUMBER}
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    sleep 10
                    curl --fail http://localhost:8082/api/health
                '''
            }
        }
    }

    post {
        success {
            echo 'Employee Portal CI/CD Pipeline Completed Successfully!'
        }

        failure {
            echo 'Employee Portal CI/CD Pipeline Failed!'
        }
    }
}

