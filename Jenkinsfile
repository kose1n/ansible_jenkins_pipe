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
                    sh 'docker push kose1n/k8_docker:${DOCKER_TAG}'
                }
            }
        }
    }
}

def getDockerTag(){
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
