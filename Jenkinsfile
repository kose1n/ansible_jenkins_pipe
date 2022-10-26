pipeline {
    agent any
    environment {
        DOCKER_TAG = getDockerTag()
    }
    stages {
        stage('Build Docker Image'){
            steps{
                sh "docker build . -t kammana/nodeapp:${DOCKER_TAG} "
            }
        }
        stage("docker login") {
            steps {
                echo " ============== docker login and push =================="
                withCredentials([usernamePassword(credentialsId: 'dockerhub_kose1n', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh "docker login -u $USERNAME -p $PASSWORD"
                }
            }
        }
        stage('create docker image') {
            steps {
                   echo ' ============== start building image =================='
                   dir ('docker/k8_docker') {
                         sh 'docker build -t kose1n/k8_docker:latest . '
                   }
            }
        }
        stage("docker push") {
            steps {
                echo " ============== start pushing image =================="
                sh '''
                docker push kose1n/k8_docker:latest
                '''
            }
        }
    }
}

def getDockerTag(){
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
