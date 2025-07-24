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

        stage('Tes
