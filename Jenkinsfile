pipeline {
    agent any

    tools {
        maven 'Maven_3.9'
        jdk 'JDK21'
    }

    environment {
        // SonarQube server name (as configured in Jenkins > Manage Jenkins > System)
        SONARQUBE_ENV = 'SonarQube'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your main branch
                git branch: 'main', url: 'https://github.com/ShranyaRudraksha/Jenkins.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building project using Maven..."
                // Clean and build the project
                bat 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Running SonarQube static analysis..."
                withSonarQubeEnv("${SONARQUBE_ENV}") {
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
            echo "✅ Build and analysis completed successfully!"
        }
        failure {
            echo "❌ Build failed. Check logs and SonarQube dashboard for details."
        }
    }
}
