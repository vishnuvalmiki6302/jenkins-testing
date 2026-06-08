pipeline {

    agent any

    environment {

        FRONTEND_IMAGE = "frontend-nginx"
        BACKEND_IMAGE  = "backend-node"

        TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Source') {

            steps {

                checkout scm

            }
        }

        stage('Build Frontend Image') {

            steps {

                dir('frontend') {

                    sh """
                    docker build -t ${FRONTEND_IMAGE}:${TAG} .
                    """

                }

            }

        }

        stage('Build Backend Image') {

            steps {

                dir('backend') {

                    sh """
                    docker build -t ${BACKEND_IMAGE}:${TAG} .
                    """

                }

            }

        }

        stage('Verify Images') {

            steps {

                sh '''
                docker images
                '''

            }

        }

        stage('Remove Old Containers') {

            steps {

                sh '''
                docker rm -f frontend-container || true
                docker rm -f backend-container || true
                '''

            }

        }

        stage('Deploy Frontend') {

            steps {

                sh """
                docker run -d \
                --name frontend-container \
                -p 8082:80 \
                ${FRONTEND_IMAGE}:${TAG}
                """

            }

        }

        stage('Deploy Backend') {

            steps {

                sh """
                docker run -d \
                --name backend-container \
                -p 3000:3000 \
                ${BACKEND_IMAGE}:${TAG}
                """

            }

        }

        stage('Verify Deployment') {

            steps {

                sh '''
                docker ps
                '''

            }

        }

    }

    post {

        success {

            echo 'Deployment Successful'

        }

        failure {

            echo 'Deployment Failed'

        }

        always {

            echo 'Pipeline Completed'

        }

    }

}