pipeline {
    agent any

    parameters {
        string(name: 'DOCKER_IMAGE_NAME', defaultValue: 'car-image', description: 'Name of the Docker image')
    }

    environment {
       AWS_ACCESS_KEY_ID = credentials('jen4')
    }

    stages {
        stage('Checkout Application Code') {
            steps {
                script {
                    // Checkout application code from the specified Git repository
                
               checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'git-pass', url: 'https://github.com/shanilarej/carweb-project.git']])
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh "  docker build -t ${params.DOCKER_IMAGE_NAME}:latest -f Dockerfile ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Assuming 'dockerhub-devops' is the ID of your Docker Hub credentials
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-devops', passwordVariable: 'pass2', usernameVariable: 'user2')]) {
    // some block

                        // Log in to Docker Hub using provided credentials
                        sh "echo '${pass2}' | docker login -u ${user2} --password-stdin"
                        
                        // Tag and push the built Docker image to Docker Hub
                        sh "docker tag ${params.DOCKER_IMAGE_NAME}:latest ${user2}/${params.DOCKER_IMAGE_NAME}:latest"
                        sh "docker push ${user2}/${params.DOCKER_IMAGE_NAME}:latest"
                    }
                }
            }
        }

      /*  stage('Deployment to EKS-cluster') {
            steps {
                script {
                    withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'EKS-K8', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                        sh 'kubectl get nodes'
                        sh 'kubectl apply -f car-deploy.yaml'
                        sh 'kubectl apply -f car-svc.yaml'
                        sh 'kubectl get svc canary-svc -o wide'
                        sh 'kubectl scale deployment blue-app --replicas=3'

                    }
                }
            } 
        }*/
    }
}
