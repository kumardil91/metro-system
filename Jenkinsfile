node { 
   stage('SCM Checkout') {
    git 'https://github.com/kumardil91/spring-boot-maven-example-helloworld'
   }
   stage('Build'){
   //Get maven home path 
   def mvnHome =  tool name: 'maven-3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
   }
   stage('Test') {
        sh '${mvnHome}/bin/mvn test -B'
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
}
