@Library('my-shared-library') _
pipeline{
  agent any
  parameters {
  choice choices: ['create delete'], description: 'for creating and destroying pods', name: 'action'
  }
  stages{
    stage('Git Checkout'){
      when { expression { params.action == 'create' }}
        steps{ 
          script{
               gitCheckout(
                 branch:'main',
                 url: 'https://github.com/santanubose03/mrdevops_java_app.git'
               )
             }
           }
       }
    
     stage('Unit Testing'){
       when { expression { params.action == 'create' }}
        steps{ 
          script{
               mvnTest()
             }
           }
       }
     stage('Maven Integration Testing'){
      when { expression { params.action == 'create' }}
        steps{ 
          script{
               mvnIntegrationTest()
             }
           }
       }
    stage('Maven Build '){
      when { expression { params.action == 'create' }}
        steps{ 
          script{
               mvnBuild()
             }
           }
       }
    }
  post {
            always {
             script {
                // Delete all files and subdirectories in the workspace
                //def workspace = pwd()  // Get the current workspace path
                //println "workspace path is : ${workspace}"
                //sh "rm -rf ${workspace}/*"
              // bat "rmdir /S /Q ."
              // deleteDir()
               cleanWs()
            }
        }
      }
}
