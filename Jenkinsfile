node {
   def mvnHome
   stage('Prepare') {
      git url: 'git@github.com:nandudemy/devops.git', branch: 'develop'
      mvnHome = tool 'maven'
   }
   stage('Build') {
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Unit Test') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
   stage('Integration Test') {
     if (isUnix()) {
        sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean verify"
     } else {
        bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean verify/)
     }
   }
   stage('Sonar') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' sonar:sonar"
      } else {
         bat(/"${mvnHome}\bin\mvn" sonar:sonar/)
      }
   }
   if(env.BRANCH_NAME == 'develop'){
     stage("Upload"){
        // Artifact repository upload steps here
        echo 'Uploaded snapshot to artifactory'
     }
     stage("Deploy"){
        // Deploy steps here
        echo 'Deployed snapshot to DEV'
     }
     stage("Smoke Test"){
         echo 'Deployed snapshot to DEV'
     }
   }
   if(env.BRANCH_NAME ==~ /release.*/){
     stage("Deploy"){
        echo 'Deployed release to DEV'
     }
     stage("Dev Approval"){
        echo 'Waiting on Approval'
     }
     stage("Release"){
        echo 'Uploaded release version to artifactory and git commit'
     }
     stage("Promoting build to QA"){
        echo 'Releasing release version to QA artifactory'
     }
     stage("Approval QA Deploy"){
        echo 'Release build from QA artifactory to QA'
     }
   }

}