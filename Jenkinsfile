pipeline {
    agent any

    tools {
        maven 'M2_HOME'       // Must match the Maven tool name in Jenkins
        jdk 'JAVA_HOME'       // Must match the JDK tool name in Jenkins
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
                    // Auto-detect the jar file name (excluding original- jars)
                    def jarFile = bat(
                        script: 'for /f "delims=" %%f in (\'dir /b target\\*.jar ^| findstr /v "original"\') do @echo %%f',
                        returnStdout: true
                    ).trim()

                    echo "Detected JAR: ${jarFile}"

                    // Run the Spring Boot app on port 8991
                    bat "start java -jar target\\${jarFile} --server.port=8991"
                }
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Additional deployment steps can go here.'
                // For example: upload to a server, call webhook, etc.
            }
        }
    }

    post {
        success {
            echo 'Build, test, and deployment succeeded!'
        }
        failure {
            echo 'Build or deployment failed.'
        }
    }
}
