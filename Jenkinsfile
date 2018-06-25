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
   stage('Development deploy') {
   def jarName = "application-*.jar"
   build job: "ApplicationToDev", parameters: [[$class: 'StringParameterValue', name: 'jarName', value: jarName]]
      echo 'the application is deployed !'
   }
    
}
