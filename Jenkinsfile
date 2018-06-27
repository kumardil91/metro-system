node { 
   stage('SCM Pull') {
    git 'https://github.com/kumardil91/metro-system'
   }
   stage('Build'){
   //Get maven home path 
   def mvnHome =  tool name: 'maven-3', type: 'maven'   
      sh "${mvnHome}/bin/mvn -Dmaven.test.skip=true clean package"
   }
   
   stage('Test') {
      def mvnHome =  tool name: 'maven-3', type: 'maven'
        sh "${mvnHome}/bin/mvn test -B"
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
      
   }
   stage('Slack Notification'){
    slackSend baseUrl: 'https://necect.slack.com/services/hooks/jenkins-ci/', channel: 'jenkinspipeline', color: 'Red', message: 'Slack Notification', teamDomain: 'necect', tokenCredentialId: 'slackjen'

   }
    
}
