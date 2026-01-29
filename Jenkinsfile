pipeline {
    agent any

    stages {

        stage('Clone Repository') {
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
                sh '''
                docker run -d \
                --name genai-container \
                --env-file .env \
                -p 8000:8000 \
                genai-app:latest
                '''
            }
        }
    }
}
