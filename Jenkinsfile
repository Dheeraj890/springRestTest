pipeline {
    agent any

    tools {
        maven 'M2_HOME'       // Must match the Maven name in Jenkins
        jdk 'JAVA_HOME'       // Must match the JDK name in Jenkins
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

        stage('Deploy JAR') {
            steps {
                script {
                    // Automatically detect the generated JAR name
                    def jarFile = bat(
                        script: 'for /f "delims=" %f in (\'dir /b target\\*.jar ^| findstr /v "original"\') do @echo %f',
                        returnStdout: true
                    ).trim()

                    echo "Running Spring Boot JAR: ${jarFile}"

                    // Run the JAR on port 8991 in background
                    bat "start java -jar target\\${jarFile} --server.port=8991"
                }
            }
        }
    }

    post {
        success {
            echo 'Build, Test and Deployment completed successfully!'
        }
        failure {
            echo 'Build or deployment failed.'
        }
    }
}
