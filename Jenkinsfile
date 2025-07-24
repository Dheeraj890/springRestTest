pipeline {
    agent any

    tools {
        maven 'Maven 3.9.11' // Ensure this Maven version is configured in Jenkins Global Tool Configuration
        JAVA_HOME 'jdk-17.0.2'       // Use the JDK name defined in Jenkins tools
    }

    environment {
        // Optional environment variables
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
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploy step - add deployment scripts here'
                // sh './deploy.sh' or upload artifacts
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
