pipeline{
    agent{ label 'dev_agent' }
    
    stages{
        stage('code'){
            steps {
                script{
                    properties([pipelineTriggers([pollSCM('')])])
                }
                git url: 'https://github.com/uzair3399/node-todo-cicd.git', branch: 'master'
            }
        }
        stage('Build and test'){
            steps {
               sh "docker build . -t uzairbagwan/note-todo-app-cicd:latest"
            }
        } 
        stage('login and Push Image'){
            steps {
               echo "logging into docker-hub and pushing"
               withCredentials([usernamePassword(credentialsId:'dockerhub',passwordVariable:'dockerhubPassword',usernameVariable:'dockerhubUser')]){
                   sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPassword}"
                   sh "docker push uzairbagwan/note-todo-app-cicd:latest"
            }   
        }
             
    }    
        stage('Deploy'){
            steps {
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }        
}
