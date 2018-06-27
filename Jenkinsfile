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
       slackSend baseUrl: 'https://necect.slack.com/services/hooks/jenkins-ci/',
	   token : uAKCcZp0tG70EBcC6yq3uACz
       channel: "#jenkinspipeline",
       color: "red", 
       message: "Welcome to Jenkins, Slack!"
 }
 
      
    
}
