
pipeline {
    agent any

    options {
        timestamps()
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code is being checked out from GitHub'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Checking the website files'
                bat 'if not exist index.html exit /b 1'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing website content'
                bat 'findstr /C:"Hello from Jenkins CI/CD!" index.html >nul'
            }
        }

        stage('Package') {
            steps {
                echo 'Creating website package'
                bat 'powershell -NoProfile -ExecutionPolicy Bypass -Command "Compress-Archive -Path index.html -DestinationPath website.zip -Force"'
                archiveArtifacts artifacts: 'website.zip', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                echo 'Copying website to deployment folder'
                bat 'if not exist C:\\cicd-deploy mkdir C:\\cicd-deploy'
                bat 'copy /Y index.html C:\\cicd-deploy\\index.html'
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Website pipeline completed!'
        }

        failure {
            echo 'FAILED: Check Console Output for errors.'
        }
    }
}
