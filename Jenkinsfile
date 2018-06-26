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
  stage('Create Image Builder') {
      when {
        expression {
          openshift.withCluster() {
            return !openshift.selector("bc", "metro-system").exists();
          }
        }
      }
      steps {
        script {
          openshift.withCluster() {
            openshift.newBuild("--name=metro-system", "--image-stream=redhat-openjdk18-openshift:1.1", "--binary")
          }
        }
      }
    }
    stage('Build Image') {
      steps {
        script {
          openshift.withCluster() {
            openshift.selector("bc", "metro-system").startBuild("--from-file=target/metro-system*.jar", "--wait")
          }
        }
      }
    }
    stage('Promote to DEV') {
      steps {
        script {
          openshift.withCluster() {
            openshift.tag("metro-system:latest", "metro-system:dev")
          }
        }
      }
    }
    stage('Create DEV') {
      when {
        expression {
          openshift.withCluster() {
            return !openshift.selector('dc', 'metro-system-dev').exists()
          }
        }
      }
      steps {
        script {
          openshift.withCluster() {
            openshift.newApp("metro-system:latest", "--name=metro-system-dev").narrow('svc').expose()
          }
        }
      }
    }
    
}
