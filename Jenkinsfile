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

        stage('Push to Docker Hub') {
            steps {
                // Ensure you have added 'docker_hub_creds' in Jenkins Credentials
                withCredentials([usernamePassword(credentialsId: 'docker_hub_creds', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    sh 'docker push vaishu09/secure-hybrid:v1'
                }
            }
        }

        stage('Deploy to K8s') {
            steps {
                // This triggers the deployment on VM3
                // Ensure Jenkins has 'kubectl' configured or uses SSH to VM3
                sh 'kubectl apply -f deployment.yaml'
                echo 'Application deployed to Kubernetes!'
            }
        }
    }
}
