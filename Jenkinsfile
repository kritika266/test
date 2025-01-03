pipeline {
    agent any
 
    environment {
        PYTHON_PATH = 'C:\Program Files\Python312;C:\Program Files\Python312\Scripts'
        SONAR_SCANNER_PATH = 'C:\Users\HP\Downloads\sonar-scanner-cli-6.2.1.4610-windows-x64\sonar-scanner-6.2.1.4610-windows-x64\bin'
    }
 
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
 
        stage('Build') {
            steps {
                // Set the PATH and install dependencies using pip
                bat '''
                set PATH=%PYTHON_PATH%;%PATH%
                pip install -r requirements.txt
                '''
            }
        }
 
        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token') // Accessing the SonarQube token stored in Jenkins credentials
            }
            steps {
                // Ensure that sonar-scanner is in the PATH
                bat '''
                set PATH=%SONAR_SCANNER_PATH%;%PATH%
                where sonar-scanner || echo "SonarQube scanner not found. Please install it."
                set PATH=%PYTHON_PATH%;%PATH%
                sonar-scanner -Dsonar.projectKey=std ^
                    -Dsonar.sources=. ^
                    -Dsonar.host.url=http://localhost:9000 ^
                    -Dsonar.token=%SONAR_TOKEN%
                '''
              
            }
        }
    }
 
    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }
}
