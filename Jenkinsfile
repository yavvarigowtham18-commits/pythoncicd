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
                bat 'python -m venv .venv'
                bat '.venv\\Scripts\\python.exe -m pip install --upgrade pip'
                bat '.venv\\Scripts\\python.exe -m pip install -r requirements.txt'
            }
        }

        stage('Compilation') {
            steps {
                echo 'Compiling Python source code'
                bat '.venv\\Scripts\\python.exe -m compileall -q app.py tests'
            }
        }

        stage('Automated Testing') {
            steps {
                echo 'Running Python test cases'
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
                echo 'Packaging Python application'
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
