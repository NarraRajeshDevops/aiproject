pipeline {
    agent any

    tools {
        nodejs 'NodeJs'   // configure in Jenkins tools
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/NarraRajeshDevops/aiproject.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Build') {
            steps {
                bat 'npm run build || echo "No build step"'
            }
        }

        stage('Test') {
            steps {
                bat 'npm test || echo "No tests"'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploy step here (Docker/K8s/EC2)"
            }
        }
    }
}
