pipeline {
    agent any

    tools {
        maven 'Maven_3.9'
        jdk 'JDK21'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ShranyaRudraksha/Jenkins.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building project using Maven..."
                bat 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Running SonarQube static analysis..."
                withSonarQubeEnv('MySonarQube') {
                    bat '''
                        mvn sonar:sonar ^
                        -Dsonar.projectKey=jenkins-demo ^
                        -Dsonar.host.url=http://localhost:9000
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo "Waiting for SonarQube Quality Gate result..."
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo "✅ Build and SonarQube analysis passed!"
        }
        failure {
            echo "❌ Build failed. Check logs and SonarQube dashboard for details."
        }
    }
}
