pipeline {
    agent any

    tools {
        maven 'Maven 3' // Configure this in Jenkins > Global Tool Configuration
        jdk 'JDK 17'    // Same for JDK
    }

    environment {
        DOCKER_IMAGE = 'spring-petclinic:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/spring-projects/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker run -d -p 8081:8080 --name petclinic $DOCKER_IMAGE'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
