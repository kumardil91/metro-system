pipeline {
stages { 
   stage('SCM Pull') {
    git 'https://github.com/kumardil91/metro-system'
   }
   stage('Build'){
   //Get maven home path 
   def mvnHome =  tool name: 'maven-3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
   }
   stage('Test') {
    def mvnHome =  tool name: 'maven-3', type: 'maven'
        sh '${mvnHome}/bin/mvn test -B'
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
   }
   post {
   success {
      slackSend baseUrl: 'https://necect.slack.com/services/hooks/jenkins-ci/', channel: 'jenkinspipeline', color: 'Good', message: 'Build Successful', teamDomain: 'necect', tokenCredentialId: 'slackjen'
   }
   failure {
        slackSend baseUrl: 'https://necect.slack.com/services/hooks/jenkins-ci/', channel: 'jenkinspipeline', color: 'Good', message: 'Build failed', teamDomain: 'necect', tokenCredentialId: 'slackjen'
   }
   }
   }
