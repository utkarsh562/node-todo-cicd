pipeline{
    agent{ label 'Dev-agent' }
    
    stages{
        
        stage("code") {
            
           steps{
               script{
                   properties([pipelineTriggers([pollSCM(' ')])])
               }
                git url: 'https://github.com/utkarsh562/node-todo-cicd.git' , branch: 'master'
           }
        }
        stage('Build and test')
        {
            steps{
                sh 'docker build . -t utkarshpathak/node-todo-app-cicd:latest'
            }
        }
        
        stage('push an image'){
            steps{
               echo "Pushing an image"
               withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pass', usernameVariable: 'user')]) {
                   sh "docker login --username=${user} --password=${pass}"
                   sh 'docker push  utkarshpathak/node-todo-app-cicd:latest '
                   
               }
                    
       
            }
            
        }
        stage( ' deployment '){
            steps{
               sh "docker-compose down && docker-compose up -d"
            }
        }
        
    }
}
