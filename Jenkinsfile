node { 
   stage('SCM Pull') {
    git 'https://github.com/kumardil91/metro-system'
   }
   stage('Build'){
   //Get maven home path 
   def mvnHome =  tool name: 'maven-3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean compile"
   }
   stage('Test') {
      def mvnHome =  tool name: 'maven-3', type: 'maven'
        sh "${mvnHome}/bin/mvn test -B"
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
      def java = scanForIssues tool: [$class: 'Java']
        def javadoc = scanForIssues tool: [$class: 'JavaDoc']
         
        publishIssues issues:[java]
        publishIssues issues:[javadoc]
   }
    stage ('Analysis') {
        def mvnHome = tool name: 'maven-3', type: 'maven'
 
        sh "${mvnHome}/bin/mvn -batch-mode -V -U -e checkstyle:checkstyle pmd:pmd pmd:cpd findbugs:findbugs "
 
        def checkstyle = scanForIssues tool: [$class: 'CheckStyle'], pattern: '**/target/checkstyle-result.xml'
        publishIssues issues:[checkstyle]
    
        def pmd = scanForIssues tool: [$class: 'Pmd'], pattern: '**/target/pmd.xml'
        publishIssues issues:[pmd]
         
        def cpd = scanForIssues tool: [$class: 'Cpd'], pattern: '**/target/cpd.xml'
        publishIssues issues:[cpd]
         
        def findbugs = scanForIssues tool: [$class: 'FindBugs'], pattern: '**/target/findbugsXml.xml'
        publishIssues issues:[findbugs]
 
    }
}
