pipeline {
    agent any

    tools {
        nodejs "Node18"   // name must match the NodeJS tool configured in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                echo "📦 Cloning repository..."
                git url: 'https://github.com/Sai4224/Website.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "⚙️ Installing npm packages..."
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                echo "🏗️ Building project..."
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Deploying frontend build..."
                // Example: copy files to a web server folder
                sh '''
                    rm -rf /var/www/html/*
                    cp -r build/* /var/www/html/
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}
