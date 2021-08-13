// This jenkins file clone code from git pubilc repo and do the static code analysis ans send the report in mail from jenkins . 
//i did mail config in sonar so if any change is happend in code mail is also send from sonar.
FAILED_STAGE=env.STAGE_NAME
pipeline {
  agent any
   stages {
    stage('clone') {
      steps {
	      FAILED_STAGE=env.STAGE_NAME
        git 'https://github.com/dhana1993/1ant.git'
        
      }
    }
    stage('Code Quality Check via SonarQube') {
        steps {
            script {
		    FAILED_STAGE=env.STAGE_NAME
                
                withSonarQubeEnv("SonarQube") {
                    bat ''' D:\\sonar-scanner-4.6.1.2450-windows\\bin\\sonar-scanner -Dsonar.projectKey=gameoflife -Dsonar.source=.  -Dsonar.java.binaries=src'''
                 
                  /*  //also we can write below formates
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
	       FAILED_STAGE=env.STAGE_NAME
	       // sonar qube reprt -task.txt present in .scannerwork\\report-task.txt this path
           bat '''  move .scannerwork\\report-task.txt . '''
	    // type    report-task.txt      	// to read a file
	   // del /f report-task.txt            // delete file in windows batch command
       }
   }
   
  post {
   success {
    script {
               if (fileExists('report-task.txt')) {
		       
		        echo "############################ Attachment is existed ####################"
                    emailext attachmentsPattern: '**/report-task.txt',  
                      body: ' Hi team   \n BUILD URL : ${BUILD_URL} \n  Build number: ${BUILD_NUMBER} \n Build status: ${currentBuild.result} \n SonarQube result URL: ${BUILD_LOG_REGEX, regex=".*ANALYSIS SUCCESSFUL, you can browse (.*)", showTruncatedLines=false, substText="$1"\n Find attachment related to sonarqube', 
                      subject: 'Regarding ${JOB_NAME} jenkins job   ',
                     // recipientProviders: [[$class: 'CulpritsRecipientProvider'],[$class: 'RequesterRecipientProvider']],
                      to: 'lakshmibalanagu9602@gmail.com'
               }
               else {
                    echo "############################ Unable to find the attachment ####################"
                    emailext  body: ' Hi team  \n Build success but unable to find the sonarqube report \n BUILD URL : ${BUILD_URL} \n Build status: ${currentBuild.result}', 
                      subject: 'Jenkins job name is ${JOB_NAME}', 
                      to: 'lakshmibalanagu9602@gmail.com'
               }
       
          }
	     }
	  failure {
            emailext    body: ' Hi team   \n build is failed in  ${FAILED_STAGE} stage \n BUILD URL : ${BUILD_URL}  \n ',
                        to: "${EMAIL_TO}",
                        subject: 'Jenkins build is success : $PROJECT_NAME - #$BUILD_NUMBER '
        }
   }
}
