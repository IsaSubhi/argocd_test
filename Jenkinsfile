pipeline {
    agent any

    environment {
        ARGOCD_FILE = 'argocd_app.yaml'
        DOCKERHUB_REPO = 'isasubhi/argocd-app'
        GIT_REPO = 'git@github.com:IsaSubhi/argocd_test.git'
        VERSION = '1.2'
    }

    stages {
        stage('1-Clone Git Repository') {
            steps {
                sh "git init"
                sh "git pull ${GIT_REPO}"
            }
        }
        
        stage('2-Build Docker Image') {
            steps {
                sh """
                echo 'FROM nginx:latest' > Dockerfile
                echo 'COPY index.html /usr/share/nginx/html/index.html' >> Dockerfile
                """
                sh "docker build -t ${DOCKERHUB_REPO}:${VERSION} ."
            }
        }
        
        stage('3-Push Image to Dockerhub') {
            steps {
                sh "docker login"
                sh "docker push ${DOCKERHUB_REPO}:${VERSION}"
            }
        }
        
        stage('4-Deploy to Kubernetes') {
            steps {
                sh "kubectl apply -f ${ARGOCD_FILE}"
            }
        }
    }
}

