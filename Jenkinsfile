pipeline {
    agent any
    environment {
        SOURCE_REPO = 'https://github.com/NarraRajeshDevops/aiproject.git'
        TARGET_REPO = 'https://github.com/NarraRajeshDevops/aiproject-destination.git'
        GIT_CREDENTIALS_ID = 'Jenkinskey'  // Replace with your Jenkins credential ID
        SOURCE_DIR = 'source_repo'
        TARGET_DIR = 'target_repo'
    }
    stages {

        stage('Clean Workspace') {
            steps {
                echo 'Cleaning workspace...'
                deleteDir() // start fresh
            }
        }

        stage('Checkout Source Repo') {
            steps {
                echo "Cloning source repo..."
                dir("${SOURCE_DIR}") {
                    git branch: 'main',
                        url: "${SOURCE_REPO}",
                        credentialsId: "${GIT_CREDENTIALS_ID}"
                }
            }
        }

        stage('Prepare Target Repo') {
            steps {
                script {
                    // If target folder exists locally, just pull updates
                    if (fileExists("${TARGET_DIR}/.git")) {
                        echo "Target repo exists locally, pulling latest changes..."
                        dir("${TARGET_DIR}") {
                            bat """
                                git checkout main
                                git pull origin main
                            """
                        }
                    } else {
                        echo "Target repo does not exist locally, cloning..."
                        dir("${TARGET_DIR}") {
                            git branch: 'main',
                                url: "${TARGET_REPO}",
                                credentialsId: "${GIT_CREDENTIALS_ID}"
                        }
                    }
                }
            }
        }

        stage('Copy Source to Target') {
            steps {
                echo "Copying code from source to target..."
                bat """
                    xcopy /E /Y /I ${SOURCE_DIR}\\* ${TARGET_DIR}\\
                    del ${TARGET_DIR}\\.git /Q /S || echo "Keep .git folder"
                """
            }
        }

        stage('Commit & Push') {
            steps {
                dir("${TARGET_DIR}") {
                    echo "Committing and pushing changes..."
                    bat """
                        git add .
                        git commit -m "Jenkins: Copy/Update code from source repo" || echo "Nothing to commit"
                        git push origin main
                    """
                }
            }
        }

    }
    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Code copied/updated successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs.'
        }
    }
}
