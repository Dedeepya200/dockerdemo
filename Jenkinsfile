pipeline{
    agent any
    stages{
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp'
            }
        }
        stage('Run Docker Container') {
            steps {
                sh 'docker rm -f mycontainer || exit0'
                sh 'docker run -d -p 5001:5001 --name mycontainer myapp'
            }
        }
    }
    post {
        success{
            echo 'build , run done successfully'
        }
        failure{
            echo 'failed building'
        }
    }
}