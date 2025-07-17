pipeline {
    agent any

    tools {
        maven 'Maven3.8.8'         // Use the name you defined in Jenkins Global Tool Configuration
    }

    environment {
        SONAR_SCANNER_HOME = tool 'SonarScanner'  // The SonarQube Scanner name from Global Tool Config
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('MySonarQube') {
                    sh """
                        ${SONAR_SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=hello-world-java \
                        -Dsonar.sources=src \
                        -Dsonar.java.binaries=target
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Run App') {
            steps {
                sh 'java -jar target/hello-world-java-1.0.0.jar'
            }
        }
    }
}
