pipeline {
    agent any   // Run on any available agent
    environment {
        SOURCE_REPO = 'https://github.com/NarraRajeshDevops/aiproject.git'
        TARGET_REPO = 'https://github.com/NarraRajeshDevops/aiproject-destination.git'
        GIT_CREDENTIALS_ID = 'your-jenkins-git-credentials-id'
        WORKSPACE_SOURCE = 'source_repo'
        WORKSPACE_TARGET = 'target_repo'
    }
    stages {

        stage('Clean Workspace') {
            steps {
                echo 'Cleaning workspace...'
                deleteDir() // clean everything to avoid conflicts
            }
        }

        stage('Checkout Source Repo') {
            steps {
                echo "Checking out source repo..."
                dir("${WORKSPACE_SOURCE}") {
                    git branch: 'main', 
                        url: "${SOURCE_REPO}", 
                        credentialsId: "${GIT_CREDENTIALS_ID}"
                }
            }
        }

        stage('Prepare Target Repo') {
            steps {
                echo "Cloning target repo..."
                dir("${WORKSPACE_TARGET}") {
                    git branch: 'main', 
                        url: "${TARGET_REPO}", 
                        credentialsId: "${GIT_CREDENTIALS_ID}"
                }
            }
        }

        stage('Copy Code') {
            steps {
                echo "Copying code from source to target..."
                // copy all files except .git from source to target
                sh """
                rsync -av --exclude='.git' ${WORKSPACE_SOURCE}/ ${WORKSPACE_TARGET}/
                """
                // for Windows agent, use xcopy instead
                // bat 'xcopy /E /Y /I ${WORKSPACE_SOURCE}\\* ${WORKSPACE_TARGET}\\'
            }
        }

        stage('Commit and Push') {
            steps {
                dir("${WORKSPACE_TARGET}") {
                    echo "Adding, committing, and pushing changes..."
                    sh '''
                    git add .
                    git commit -m "Jenkins: Copy code from source repo"
                    git push origin main
                    '''
                    // For Windows:
                    // bat 'git add .'
                    // bat 'git commit -m "Jenkins: Copy code from source repo"'
                    // bat 'git push origin main'
                }
            }
        }

    }
    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Code copied successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs.'
        }
    }
}
