
pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                echo "Building Flask Docker Image"

                sh 'docker build -t dedeepya200/flaskapphello:latest .'
            }
        }

        stage('Docker Login') {
            steps {
                echo "Logging in to Docker Hub"

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                        -u "$DOCKER_USERNAME" \
                        --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "Pushing Flask Image to Docker Hub"

                sh 'docker push dedeepya200/flaskapphello:latest'
            }
        }

        stage('Check Kubernetes Connection') {
            steps {
                echo "Checking Kubernetes Connection"

                sh 'kubectl config current-context'
                sh 'kubectl get nodes'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying Flask Application"

                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "Checking Kubernetes Resources"

                sh 'kubectl get deployments'
                sh 'kubectl get pods'
                sh 'kubectl get services'
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }

        failure {
            echo "Pipeline failed. Please check the logs."
        }
    }
}
