
pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {

        stage('Source Code Checkout') {
            steps {
                echo 'Checking out source code from GitHub'
                checkout scm
            }
        }

        stage('Python Environment Setup') {
            steps {
                bat '"C:\\Users\\SYS-64\\AppData\\Local\\Python\\pythoncore-3.15-64\\python.exe" -m venv .venv'
                bat '.venv\\Scripts\\python.exe -m pip install --upgrade pip'
                bat '.venv\\Scripts\\python.exe -m pip install -r requirements.txt'
            }
        }

        stage('Compilation') {
            steps {
                bat '.venv\\Scripts\\python.exe -m compileall -q app.py tests'
            }
        }

        stage('Automated Testing') {
            steps {
                bat '.venv\\Scripts\\python.exe -m pytest tests --junitxml=test-results.xml'
            }
            post {
                always {
                    junit allowEmptyResults: true, testResults: 'test-results.xml'
                }
            }
        }

        stage('Application Packaging') {
            steps {
                bat 'if not exist dist mkdir dist'
                bat 'powershell -NoProfile -Command "Compress-Archive -Path app.py,requirements.txt -DestinationPath dist\\python-app.zip -Force"'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
            archiveArtifacts artifacts: 'dist/*.zip', fingerprint: true
        }

        failure {
            echo 'CI/CD Pipeline failed!'
        }
    }
}
