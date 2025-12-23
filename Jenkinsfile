pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "forcex0/student-management"
        DOCKER_BUILDKIT = "0"
    }

    stages {


       stage('MVN SONARQUBE') {
            steps {
                sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.1.2184:sonar \
                    -Dsonar.host.url=http://localhost:9000 \
                    -Dsonar.token=sqa_bf2c984b82cbc07ae3879f3ec047bc468ecbf458'
            }
        }

        stage('Build Maven') {
            steps {
                sh "mvn clean package -DskipTests"
            }
        }

        
    }

}
