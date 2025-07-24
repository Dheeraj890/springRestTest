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
                    // Get the JAR filename (excluding original-*.jar files)
                    def jarFile = bat(
                        script: 'for /f "delims=" %%f in (\'dir /b target\\*.jar ^| findstr /v "original"\') do @echo %%f',
                        returnStdout: true
                    ).trim()

                    echo "Detected JAR: ${jarFile}"

                    // Run the JAR on port 8991 in background
                    bat "start java -jar target\\${jarFile} --server.port=8991"
