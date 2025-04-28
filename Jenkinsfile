pipeline {
    agent any
    environment {
        SONARQUBE_SERVER = 'SonarQube'
        DOCKER_IMAGE = 'ayamz/spring-petclinic'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/AyaMZ28/spring-petclinic.git'
            }
        }
        stage('Code Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn clean verify sonar:sonar'
                }
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Docker Build & Push') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
        stage('Deploy to K8s') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }
    post {
        success {
            emailext subject: "✅ Build Successful",
                     body: "Spring PetClinic was built and deployed successfully.",
                     to: "zakariaaya47@gmail.com"
        }
        failure {
            emailext subject: "❌ Build Failed",
                     body: "Build or deployment failed. Please check Jenkins logs.",
                     to: "zakariaaya47@gmail.com"
        }
    }
}

