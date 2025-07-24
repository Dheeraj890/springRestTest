pipeline {
    agent any

    tools {
        maven 'M2_HOME'
        jdk 'JAVA_HOME'
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
                    // Write just the JAR file name to a temp file
                    bat '''
                        @echo off
                        for /f "delims=" %%f in ('dir /b target\\*.jar ^| findstr /v "original"') do (
                            echo %%f > jarname.txt
                            goto done
                        )
                        :done
                    '''

                    // Read the jar name from file
                    def jarFile = readFile('jarname.txt').trim()

                    echo "Detected JAR: ${jarFile}"

                    // Run it on port 8991
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
