pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        DOCKER_USERNAME = 'shrinidhiupadhyaya'
        BACKEND_IMAGE = "${DOCKER_USERNAME}/mean-backend"
        FRONTEND_IMAGE = "${DOCKER_USERNAME}/mean-frontend"
        GIT_REPO = 'https://github.com/Shrinidhi972004/Discover-Dollar-Technical-Assignment.git'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "${GIT_REPO}"
            }
        }
        
        stage('Build Backend Image') {
            steps {
                dir('backend') {
                    sh "docker build -t ${BACKEND_IMAGE}:${BUILD_NUMBER} -t ${BACKEND_IMAGE}:latest ."
                }
            }
        }
        
        stage('Build Frontend Image') {
            steps {
                dir('frontend') {
                    sh "docker build -t ${FRONTEND_IMAGE}:${BUILD_NUMBER} -t ${FRONTEND_IMAGE}:latest ."
                }
            }
        }
        
        stage('Login to DockerHub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        
        stage('Push Images to DockerHub') {
            steps {
                sh "docker push ${BACKEND_IMAGE}:${BUILD_NUMBER}"
                sh "docker push ${BACKEND_IMAGE}:latest"
                sh "docker push ${FRONTEND_IMAGE}:${BUILD_NUMBER}"
                sh "docker push ${FRONTEND_IMAGE}:latest"
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'docker-compose pull'
                sh 'docker-compose up -d --force-recreate'
            }
        }
    }
    
    post {
        always {
            sh 'docker logout || true'
        }
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
