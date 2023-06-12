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

                  echo "code is coming..."
            }
        }

         stage('Cleanup of dev code'){
            when {
                branch 'dev'
            }
            steps{
                  echo "Cleaning dev branch code"

            }
        }
        stage('tigger prod job'){
            when {
                branch 'dev'
            }
         steps {
            build job: 'dev', parameters: [
                string(name: 'prod', value: env.NAME)
                ], wait: true
              }
        }
        stage('Code Checkout for prod'){
            when {
                branch 'prod'
            }
            
            steps{
                  echo "Code is coming form prod branch"

            }
        }
         stage('Cleanup of prod code'){
            when {
                branch 'prod'
            }
            steps{
                  echo "Cleaning prod branch code"

            }
         }
    }
}
