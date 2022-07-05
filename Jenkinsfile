pipeline {
  agent any
  triggers {
    pollSCM('*/5 * * * *')
  }
  stages{
       stage ('Build'){
        steps {
          sh 'mvn clean package'
        }
         post {
           success {
             echo 'Archiving...'
             archiveArtifacts artifacts:'**/target/*.war'
           }
         }
       }
       stage ('Deployments') {
         parallel{
           stage ('Deploy to Staging'){
             steps {
               sh "cp **/target/*.war /temp/tomcat/webapps"
             }
           }
           stage ('Deploy to prod') {
             steps {
               timeout(time:5, unit:'DAYS'){
                  input message:'Approve copy to Prod'
               }
               sh "cp **/target/*.war /temp/tomcat-prod/webapps"
             }     
           }
         }
       }
    }
} 
  
