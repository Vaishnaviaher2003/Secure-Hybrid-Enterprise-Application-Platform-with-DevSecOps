pipeline {
    agent any 

    stages {
        // Stage 1: Pull the latest code from GitHub
        stage('Pull Code') {
            steps {
                checkout scm
                echo 'Successfully pulled code from GitHub!'
            }
        }

        // Stage 2: Build the Docker Image
        stage('Build') {
            steps {
                // This command creates the container image
                sh 'docker build -t my-devsecops-app .'
            }
        }

        // Stage 3: Run the Container
        stage('Deploy') {
            steps {
                // This stops any old container and starts the new one on port 8080
                sh 'docker rm -f my-container || true'
                sh 'docker run -d --name my-container -p 8080:80 my-devsecops-app'
                echo 'Application is now running!'
            }
        }
    }
}
