pipeline {
    agent { label ' k8-master '}
    environment {
       
        Image_Name = "adpatidar/todo-app:$BUILD_NUMBER"

    }

    stages{
        stage('Dev Code Checkout'){
            when {
                branch 'dev'
            }
            steps{
                   
                  git url: 'https://github.com/adpatidar/Project-2.git', branch: 'dev'
            }
        }
        stage('Code Build'){
            when {
                branch 'dev'
            }
            steps{
                   sh 'docker build . -t adpatidar/todo-app:$BUILD_NUMBER'
                    
            }
        }
        
        stage('Image Push'){
            when {
                branch 'dev'
            }
            steps{
                 withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push $Image_Name"
                
                }
              
            }       
        }
        stage('Dev Manifest Update'){
            when {
                branch 'dev'
            }
            steps{
                   sh 'sed -i "s+adpatidar/todo-app:.*+adpatidar/todo-app:$BUILD_NUMBER+" deployment.yaml'
                   sh 'echo "$BUILD_NUMBER" > /home/ubuntu/project-2/workspace/dev-bid'
            }
        }
       
        stage('Deployment on k8s'){
            when {
                branch 'dev'
            }
            steps{
                 withKubeConfig([credentialsId: 'todo-dev', serverUrl: 'https://172.31.8.156:6443']) {
                 sh "kubectl apply -f deployment.yaml"
               }              
            }
        }
        
       
         stage('Dev Cleanup'){
            when {
                branch 'dev'
            }
            steps{
                  sh 'docker rmi $Image_Name'
                  sh "docker logout"
                  cleanWs()
                  
            }
        }
        
	        stage('Prod Code Checkout'){
            when {
                branch 'prod'
            }
   
            steps{
                   
                  git url: 'https://github.com/adpatidar/Project-2.git', branch: 'prod'
            }
        }
       
        stage('Prod Manifest Update'){
            when {
                branch 'prod'
            }
            steps{
                   sh 'sed -i \'s/todo-app-ns/todo-app-prod-ns/g\' deployment.yaml'
		   sh 'sed -i \'s/30009/30007/g\' deployment.yaml'
                   sh 'sed -i "s+adpatidar/todo-app:.*+adpatidar/todo-app:`cat /home/ubuntu/project-2/workspace/dev-bid`+" deployment.yaml'
            }
        }
       
        stage('Code Deploy'){
            when {
                branch 'prod'
            }
            steps{
                 withKubeConfig([credentialsId: 'todo-prod', serverUrl: 'https://172.31.8.156:6443']) {
                 sh "kubectl apply -f deployment.yaml"
		 }              
            }
        }
        
         stage('Prod Cleanup'){
            when {
                branch 'prod'
            }
            steps{
                  
                 cleanWs()
                 
            }
         }

    }
}
