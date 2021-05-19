pipeline {
  agent any
   stages {
    stage('clone') {
      steps {
        git 'https://github.com/dhana1993/1ant.git'
        
      }
    }
    stage('Code Quality Check via SonarQube') {
        steps {
            script {
                
                withSonarQubeEnv("SonarQube") {
                    bat ''' D:\\sonar-scanner-4.6.1.2450-windows\\bin\\sonar-scanner -Dsonar.projectKey=gameoflife -Dsonar.source=.  -Dsonar.java.binaries=src'''
                 
                  /*  //also we can write below formate
                  def scannerHome = tool 'SonarQube-scanner';//(sonar scanner name from global tool config)
                  withSonarQubeEnv("SonarQube") { // sonar server name from config sys
                  bat ''' scannerHome\\bin\\sonar-scanner \           // (or) bat ''' ${tool("SonarQube-scanner")}/bin/sonar-scanner \
                  -Dsonar.projectKey=test-node-js \
                  -Dsonar.sources=. \
                  -Dsonar.css.node=. \
                  -Dsonar.host.url=http://your-ip-here:9000 \
                  -Dsonar.login=your-generated-token-from-sonarqube-container" */

                  // execute sonar using sonar-project.properties file
                  //bat ''' D:\\sonar-scanner-4.6.1.2450-windows\\bin\\sonar-scanner -Dproject.settings=sonar-project.properties '''
                    }
                }
            }
        }
   
   stage(' moving sonar report to current directory'){
       steps{
           bat '''  move .scannerwork\\report-task.txt .  '''
       }
   }
   
  post {
   success {
    script {
               if (fileExists('report-task.txt')) {
                    
                    emailext attachmentsPattern: '**/report-task.txt',  
                      body: ' Hi team   \n BUILD URL : ${BUILD_URL} \n  Build number: ${BUILD_NUMBER} \n Build status: ${currentBuild.result} \n Find attachment related to sonarqube', 
                      subject: 'Regarding ${JOB_NAME} jenkins job   ',
                     // recipientProviders: [[$class: 'CulpritsRecipientProvider'],[$class: 'RequesterRecipientProvider']],
                      to: 'lakshmibalanagu9602@gmail.com'
               }
               else {
                    
                    emailext  body: ' Hi team  \n Build success but unable to find the sonarqube report \n BUILD URL : ${BUILD_URL} \n Build status: ${currentBuild.result}', 
                      subject: 'Jenkins job name is ${JOB_NAME}', 
                      to: 'lakshmibalanagu9602@gmail.com'
               }
       
          }
	     }
   }
}
