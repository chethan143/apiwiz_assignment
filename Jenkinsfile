pipeline{
    agent any
    stages{
        stage("Git Checkout"){
            steps{
                git url:"https://github.com/chethan143/apiwiz_assignment"
            }
        }
        //stage("Maven Package"){
           // steps{
             //   AS of now it is dummy bcz I dont have src code
                //sh "mvn clean package"
           // }
       // }
        stage("Docker Build"){
            steps{
                sh "docker build -t chethankumarhn/myweb:${env.BUILD_NUMBER} ."
            }
        }
        stage("DockerHub Push"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pwd', usernameVariable: 'user')]) {
                    sh "docker login -u ${user} -p ${pwd}"
                    sh "docker push chethankumarhn/myweb:${env.BUILD_NUMBER}"
                }
                
            }
        }
        stage("Docker Deploy Dev"){
            steps{
                sshagent(['docker-dev-ssh']) {
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@xx.xx.xx.xx docker rm -f hariapp"
                    sh "ssh ec2-user@xx.xx.xx.xx docker run -d -p 80:80 --name myweb chethankumarhn/myweb:${env.BUILD_NUMBER}"
                }
                
            }
        }
    }
}
