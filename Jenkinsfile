pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "forcex0/student-management"
        DOCKER_BUILDKIT = "0"
    }

    stages {

        stage('Build Maven') {
            steps {
                sh "mvn clean package -DskipTests"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_cred',
                    usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]) {
                    sh "echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push $DOCKER_IMAGE"
            }
        }

        stage('Deploy MySQL on K8s') {
            steps {
                sh '''
                  kubectl apply -f k8s/mysql.yaml -n devops
                '''
            }
        }

        stage('Deploy Spring Boot on K8s') {
            steps {
                sh '''
                  kubectl apply -f k8s/spring.yaml -n devops
                  kubectl rollout restart deployment spring-app -n devops
                '''
            }
        }
    }

    post {
        always {
            sh "docker logout"
        }
    }
}
