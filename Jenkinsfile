@Library('my-shared-library') _
pipeline{
  agent any
  parameters {
  choice (name: 'action', choices: ['create','delete'], description: 'for creating and destroying pods')
  String (name:'imageName',description: 'Name of docker build image', defaultValue:'javaapp')
  String (name:'imageTag',description: 'Tag of docker build image', defaultValue:'v1')
  String (name:'appName',description: 'name of Application', defaultValue:'springboot')
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
    stage('Static Code Analysis: Sonar'){
      when { expression { params.action == 'create' }}
        steps{ 
          script{
               staticCodeAnalysis()
             }
           }
       }
    stage('Quality Gate Check: Sonarqube'){
      when { expression { params.action == 'create' }}
        steps{ 
          script{
     sleep(60)       
     timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
     def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
     if (qg.status != 'OK') {
      error "Pipeline aborted due to quality gate failure: ${qg.status}"
    }
  }
              // qualityGate()
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
    stage('Maven Build '){
      when { expression { params.action == 'create' }}
        steps{ 
          script{
               dockerBuild()
             }
           }
       }
  }
  // post {
  //           always {
  //            script {
  //               // Delete all files and subdirectories in the workspace
  //               //def workspace = pwd()  // Get the current workspace path
  //               //println "workspace path is : ${workspace}"
  //               //sh "rm -rf ${workspace}/*"
  //             // bat "rmdir /S /Q ."
  //             // deleteDir()
  //              cleanWs()
  //           }
  //       }
  //     }
}
