pipeline {
    agent { label ' k8-master '}
    environment {
       
        Image_Name = "adpatidar/todo-app:$BUILD_NUMBER"

    }

    stages{
        stage('Code Checkout'){
            steps{
                   
                  git url: 'https://github.com/adpatidar/Project-2.git', branch: 'dev'
            }
        }
        stage('Code Build'){
            steps{
                   sh 'docker build . -t adpatidar/todo-app:$BUILD_NUMBER'
                    
            }
        }
        
        stage('Image Push'){
            steps{
                 withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push $Image_Name"
                
                }
              
            }       
        }
        stage('Manifest Update'){
            steps{
                   sh 'sed -i "s+adpatidar/todo-app:.*+adpatidar/todo-app:$BUILD_NUMBER+" deployment.yaml'
                       
                   
            }
        }
       
        stage('Deployment on k8s'){
            steps{
                   withKubeConfig([credentialsId: 'todo-dev', serverUrl: 'https://172.31.8.156:6443']) {
                    sh "kubectl apply -f deployment.yaml"
                    
               }              
            }
        }
        stage('Manifest Push'){
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
            steps{
                  sh 'docker rmi $Image_Name'
                  sh "docker logout"
                  cleanWs()
                  
            }
        }
        
    }
}
