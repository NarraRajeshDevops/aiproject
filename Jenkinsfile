pipeline {
    agent any

    tools {
        nodejs 'NodeJS'   // configure in Jenkins tools
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/<your-username>/my-project.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build || echo "No build step"'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test || echo "No tests"'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploy step here (Docker/K8s/EC2)"
            }
        }
    }
}
