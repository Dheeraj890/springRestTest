pipeline {
    agent any

    tools {
        maven 'M2_HOME'    // Must match Maven tool name in Jenkins
        jdk 'JAVA_HOME'        // Must match JDK tool name in Jenkins
    }

    environment {
        MAVEN_OPTS = "-Dmaven.test.failure.ignore=false"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

         stage('Package') {
            steps {
                bat 'mvn package'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploy step - add deployment scripts here'
                // bat './deploy.bat' or upload artifacts
            }
        }
    }

    post {
        success {
            echo 'Build and tests succeeded!'
        }
        failure {
            echo 'Build or tests failed.'
        }
    }
}
