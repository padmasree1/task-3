pipeline{
    agent any
    stages{
        stage("checkout"){
            steps{
                git branch: 'main', url: 'https://github.com/padmasree1/task-3.git'
            }
        }
        stage(build){
            steps{
                sh "docker build -t padmasree123/httpd:${BUILD_NUMBER} ."
            }
        }
        stage("push the docker image"){
            steps{
                withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {
                 sh "docker login -u padmasree123 -p ${dockerhub}"
               }
               sh "docker push padmasree123/httpd:${BUILD_NUMBER}"
            }
            
        }
        stage("deploye containe in another server ")
        {
            steps{
                sshagent(['deploy_container']) {
   sh 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.45.191 docker pull padmasree123/httpd:${BUILD_NUMBER}'
sh 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.45.191 docker rm -f httpd || true'
sh 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.45.191 docker run -d -p 8082:8082 --name webserver padmasree123/httpd:${BUILD_NUMBER}'
}
            }
        }
    }
}
