pipeline {
    agent any

    environment {
        DOCKER_HUB_PASSWORD = credentials('Dockerhub_pass')
        KUBE_CONFIG_FILE = credentials('KUBE_CONFIG_FILE')
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
                sh 'docker build -t khalilsellamii/tp_k8s_app:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                // Log in to Docker Hub using your credentials
                sh 'docker login -u khalilsellamii -p $DOCKER_HUB_PASSWORD'

                // Push the built image to Docker Hub
                sh 'docker push khalilsellamii/tp_k8s_app:latest'
            }
        }

        stage('Deploy on k8s') {
            steps {
                // Connect to the k3s cluster
                sh 'export KUBECONFIG=$KUBE_CONFIG_FILE'

                // Deploy on kubernetes
                sh 'kubectl apply -f kubernetes/deployment.yaml'
                sh 'kubectl apply -f kubernetes/svc.yaml'
            }
        }

    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
    }
}