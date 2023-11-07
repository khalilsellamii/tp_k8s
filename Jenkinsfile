pipeline {
    agent any

    environment {
        DOCKER_HUB_PASSWORD = credentials('Dockerhub_pass')
        BUILD_TAG = "${BUILD_NUMBER}"

    }

    stages {
        stage('Checkout') {
            steps {
                // Check out your source code from your version control system, e.g., Git.
                sh 'rm -rf Jekins_pipeline'
                sh 'git clone https://github.com/khalilsellamii/Jekins_pipeline.git'
            }
        }

        stage('Testing') {
            steps {
                // Check out your source code from your version control system, e.g., Git.
                sh 'python3 src/test.py'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build your Docker image. Make sure to specify your Dockerfile and any other build options.
                sh 'docker build -t khalilsellamii/jenkins-pipeline:v1.${BUILD_TAG} .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                // Log in to Docker Hub using your credentials
                sh 'docker login -u khalilsellamii -p $DOCKER_HUB_PASSWORD'

                // Push the built image to Docker Hub
                sh 'docker push khalilsellamii/jenkins-pipeline:v1.${BUILD_TAG}'
            }
        }

        stage('Deploy on k8s') {
            steps {
                // Log in to Docker Hub using your credentials
                sh 'export KUBECONFIG=$KUBE_CONFIG_FILE'

                // Push the built image to Docker Hub
                sh 'kubectl apply -f kubernetes/.'
            }
        }

    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
    }
}