pipeline {
    agent { label ' dev-agnet '}
    environment {

        Image_Name = "adpatidar/todo-app:$BUILD_NUMBER"

    }

    stages{
        stage('Code Checkout'){
            steps{

                  echo "code is coming..."
            }
        }
        stage('Code Build'){
            steps{
                  echo "building"
            }
        }

        stage('Image Push'){
            steps{
                 echo "pushing image"

            }
        }
        stage('Manifest Update'){
            steps{
                   echo "updated manifest file"

            }
        }

        stage('Deployment on k8s'){
            steps{
                   echo "deploy on k8s"
            }
        }
        stage('Manifest Push'){
            steps{
                  echo "push back to github"
            }
        }

         stage('Cleanup'){
            steps{
                  echo "Cleaning"

            }
        }

    }
}
