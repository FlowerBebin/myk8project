pipeline {
    agent any

    stages {
        stage('Pull Code From GitHub') {
            steps {
                git 'https://github.com/FlowerBebin/myk8project.git'
            }
        }
        stage('Build the Docker image') {
            steps {
                sh 'sudo docker build -t newimage /var/lib/jenkins/workspace/myproject'
                sh 'sudo docker tag newimage flowerbebin/newimage:latest'
                sh 'sudo docker tag newimage flowerbebin/newimage:${BUILD_NUMBER}'
            }
        }
        stage('Push the Docker image') {
            steps {
                sh 'sudo docker image push flowerbebin/newimage:latest'
                sh 'sudo docker image push flowerbebin/newimage:${BUILD_NUMBER}'
            }
        }
        stage('Deploy on Kubernetes') {
            steps {
                sh 'kubectl apply -f /var/lib/jenkins/workspace/myproject/pod.yaml'
                sh 'kubectl rollout restart deployment loadbalancer-pod'
            }
        }
    }
}
