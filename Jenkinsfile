pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'Maven_3.9'
    }

    environment {
        APP_NAME = "MyApp"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out code from Git...'
                git branch: 'main', url: 'https://github.com/ShranyaRudraksha/Jenkins.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project using Maven...'
                bat 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'mvn test'
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging application...'
                bat 'mvn package'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                // Example: copy WAR to Tomcat (if configured)
                // bat 'copy target\\myapp.war C:\\Tomcat\\webapps\\'
                echo 'Deployment simulated (no Tomcat configured)'
            }
        }
    }

    post {
        success {
            echo "✅ Build and Deployment Successful!"
        }
        failure {
            echo "❌ Build Failed!"
        }
    }
}
