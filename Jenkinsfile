pipeline {
    agent any

    tools {
        maven 'Maven 3.8.6' //Use the name of the maven installation in Jenkins Global Tools
        jdk 'jdk-21' //Use the name of the JDK installation in Jenkins Global Tools
    }

    environment {
        SONAR_HOST_URL = 'http://localhost:9000' //Replace with your SonarQube server URL
        SONAR_TOKEN = credentials('sonar-token') //Add a Jenkins credential with ID 'sonar-token'
    }

    stages {
        stage('checkout') {
            steps {
                git credentialsId: 'github-pat', url: 'https://github.com/iamrohith3/sampleJavaProject.git'

            }
        }    

        stage('Build & Deploy') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN'
                }
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
