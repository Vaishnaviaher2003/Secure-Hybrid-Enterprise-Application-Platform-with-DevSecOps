pipeline {
    agent any 

    stages {
        stage('Pull Code') {
            steps {
                checkout scm
                echo 'Successfully pulled code from GitHub!'
            }
        }

        stage('Build') {
            steps {
                // Build the image locally
                sh 'docker build -t my-devsecops-app .'
                // Tag it for Docker Hub immediately after building
                sh 'docker tag my-devsecops-app vaishu09/secure-hybrid:v1'
            }
        }

        stage('Trivy Security Scan') {
            steps {
                echo 'Starting Security Scan...'
                // This will fail the build if CRITICAL vulnerabilities are found
                sh 'trivy image --severity CRITICAL --exit-code 1 vaishu09/secure-hybrid:v1'
            }
        }

        
