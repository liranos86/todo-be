pipeline {
    agent any
    stages {
        stage('Build stage') {
            steps {
              sh 'DOCKER_BUILDKIT=1 docker build -f docker-pipeline-be -t image-builder --target builder .'
            }
        }
        stage('Delivery stage') {
            steps {
                sh 'DOCKER_BUILDKIT=1 docker build -f docker-pipeline-be -t liranos86/todo-be:jenkins-$BUILD_NUMBER --target delivery .'
            }
        }
        stage('Cleanup') {
            steps {
                echo "Cleanup It All"
                sh 'docker system prune -f'
            }
        }
        stage('push') {
            environment {
                DOCKERHUB_CREDENTIALS=credentials('dockerhub')
            }
            steps {
                    sh 'docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW'
                    sh "docker push liranos86/todo-be:jenkins-$BUILD_NUMBER"
        }
    }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
