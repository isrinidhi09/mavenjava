pipeline {
    agent any
    tools {
        maven 'sonarmaven' // Ensure this matches the Maven configuration in Jenkins
    }
    environment {
        SONAR_TOKEN = credentials('Sonarqube-token') // Replace with your credentials ID for the SonarQube token
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-17'
        PATH = "${JAVA_HOME}\\bin;${env.PATH}"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') { // Ensure this matches your SonarQube configuration
                    bat """
                        mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=mavendemo \
                        -Dsonar.projectName='mavendemo' \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.token=sqp_446e6c4ecb8c846a5f62f3e2925097bf1d3c15a9
                    """
                }
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
