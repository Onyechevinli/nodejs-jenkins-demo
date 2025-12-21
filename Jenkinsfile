pipeline {
    agent any
    
    environment {
        APP_NAME = 'nodejs-jenkins-demo'
        DOCKER_IMAGE = 'your-dockerhub-username/nodejs-jenkins-demo'
        VERSION = "${env.BUILD_ID}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo '📦 Checking out source code...'
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                echo '📥 Installing Node.js dependencies...'
                sh 'npm ci'
            }
        }
        
        stage('Run Tests') {
            steps {
                echo '🧪 Running unit tests...'
                sh 'npm test'
            }
            post {
                always {
                    junit '**/test-results.xml' // If you configure Jest to output JUnit format
                }
            }
        }
        
        stage('Build Docker Image') {
            when {
                branch 'main'  // Only build Docker on main branch
            }
            steps {
                echo '🐳 Building Docker image...'
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:${VERSION}")
                }
            }
        }
        
        stage('Push Docker Image') {
            when {
                branch 'main'
            }
            steps {
                echo '📤 Pushing Docker image to registry...'
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
        
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo '🚀 Deploying application...'
                // Add your deployment steps here
                // Examples:
                // - Deploy to Kubernetes: sh 'kubectl apply -f k8s/'
                // - Deploy to Heroku: sh 'heroku container:push web'
                // - SSH deployment: sh './deploy.sh'
                echo '✅ Deployment would happen here'
            }
        }
    }
    
    post {
        always {
            echo '🏁 Pipeline finished. Cleaning up...'
            cleanWs()
        }
        success {
            echo '🎉 Pipeline succeeded!'
            // Optional: Send success notification
        }
        failure {
            echo '❌ Pipeline failed!'
            // Optional: Send failure notification
        }
    }
}