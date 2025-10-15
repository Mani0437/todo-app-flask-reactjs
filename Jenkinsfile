pipeline {
    agent any

    environment {
        FRONTEND_DIR = "frontend"
        DEPLOY_USER = "ubuntu"                // Change as needed
        DEPLOY_HOST = "your-server-ip"        // e.g., EC2 public IP or domain
        DEPLOY_PATH = "/var/www/todo-frontend"
        SSH_CREDENTIALS_ID = "deploy-key"     // Jenkins credentials ID (SSH key)
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "Cloning the repository..."
                git branch: 'main', url: 'https://github.com/Mani0437/todo-app-flask-reactjs.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                dir("${FRONTEND_DIR}") {
                    echo "Installing Node modules..."
                    sh 'npm install'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir("${FRONTEND_DIR}") {
                    echo "Building React app..."
                    sh 'npm run build'
                }
            }
        }

        stage('Archive Build Artifacts') {
            steps {
                echo "Archiving build artifacts..."
                archiveArtifacts artifacts: "${FRONTEND_DIR}/build/**", fingerprint: true
            }
        }

        stage('Deploy to Web Server') {
            steps {
                script {
                    echo "Deploying build files to remote server..."
                    sshagent (credentials: [SSH_CREDENTIALS_ID]) {
                        sh """
                            ssh -o StrictHostKeyChecking=no ${DEPLOY_USER}@${DEPLOY_HOST} 'mkdir -p ${DEPLOY_PATH}'
                            ssh ${DEPLOY_USER}@${DEPLOY_HOST} 'rm -rf ${DEPLOY_PATH}/*'
                            scp -r ${FRONTEND_DIR}/build/* ${DEPLOY_USER}@${DEPLOY_HOST}:${DEPLOY_PATH}/
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Frontend deployed successfully!"
        }
        failure {
            echo "❌ Deployment failed. Check logs for details."
        }
    }
}
