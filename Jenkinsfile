pipeline {
    agent any

    tools {
        maven 'Maven 3.1.1' //Use the name of the maven installation in Jenkins Global Tools
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
                configFileProvider([configFile(fileId: '708968c3-6dad-4f8f-9d31-666d5e16c6f7', variable: 'MAVEN_SETTINGS')]) {
                    bat "${MAVEN_HOME}/bin/mvn -s $MAVEN_SETTINGS clean install -DskipTests"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat "${env.SONAR_SCANNER_HOME}\\bin\\sonar-scanner.bat"
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
