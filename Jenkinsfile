pipeline {
    agent any

    stages {

        stage('Checkout Source Code') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/fullstacktraning/genai-resume-analyzer.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t genai-app:latest .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker rm -f genai-container || true'
            }
        }

        stage('Run New Container') {
            steps {
                withCredentials([string(credentialsId: 'OPENAI_API_KEY', variable: 'OPENAI_API_KEY')]) {
                    sh '''
                    docker run -d \
                    --name genai-container \
                    -e OPENAI_API_KEY=$OPENAI_API_KEY \
                    -e APP_ENV=production \
                    -p 8000:8000 \
                    genai-app:latest
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful – FastAPI is live on port 8000"
        }
        failure {
            echo "❌ Deployment Failed – Check Jenkins logs"
        }
    }
}
