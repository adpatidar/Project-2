pipeline {
    agent { label ' dev-agnet '}
    environment {
       
        Image_Name = "adpatidar/todo-app:$BUILD_NUMBER"

    }

    stages{
        stage('Code Checkout'){
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
        stage('Manifest Update'){
            when {
                branch 'dev'
            }
            steps{
                   sh 'sed -i "s+adpatidar/todo-app:.*+adpatidar/todo-app:$BUILD_NUMBER+" deployment.yaml'
                  
            }
        }
       
        stage('Deployment on k8s'){
            when {
                branch 'dev'
            }
            steps{
                echo "Deploying on dev"
                 // withKubeConfig([credentialsId: 'todo-dev', serverUrl: 'https://172.31.8.156:6443']) {
                 // sh "kubectl apply -f deployment.yaml"
               //}              
            }
        }
        stage('Manifest Push'){
            when {
                branch 'dev'
            }
            steps{
                  withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                     
                    sh "git config user.email admin@example.com"
                    sh "git config user.name example"
                    sh "git add deployment.yaml"
                    sh "git commit -m 'Updated Build $BUILD_NUMBER'"
                    sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/Project-2.git"
                  }
            }
        }
        
         stage('Cleanup'){
            when {
                branch 'dev'
            }
            steps{
                  sh 'docker rmi $Image_Name'
                  sh "docker logout"
                  cleanWs()
                  
            }
        }
        
	        stage('Code Checkout'){
            when {
                branch 'prod'
            }
   
            steps{
                   
                  git url: 'https://github.com/adpatidar/Project-2.git', branch: 'prod'
            }
        }
       
        stage('Manifest Update'){
            when {
                branch 'prod'
            }
            steps{
                   sh 'sed -i \'s/todo-app-ns/todo-app-prod-ns/g\' deployment.yaml'
                   
            }
        }
       
        stage('Code Deploy'){
            when {
                branch 'prod'
            }
            steps{
                 // withKubeConfig([credentialsId: 'todo-prod', serverUrl: 'https://172.31.8.156:6443']) {
                 // sh "kubectl apply -f deployment.yaml"
		echo "Deploying on prod"
                }              
            }
        }
        
         stage('Cleanup'){
            when {
                branch 'prod'
            }
            steps{
                  
                 cleanWs()
                 
            }
         }

    }
}

