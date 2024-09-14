pipeline{
  agent any
  //options{}
  //triggers{}
  tools {
    maven "Maven 3.9.7"
  }  
  stages {
    stage('1.GetCode'){
      steps{
        sh "echo 'cloning the latest application version' "
        git branch: 'master', url: 'https://github.com/mbwork1/maven-web-application.git'
      }
    }
    stage('2.Test+Build'){
      steps{
        sh "echo 'running JUnit-test-cases' "
        sh "echo 'testing must passed to create artifacts' "
        sh "mvn clean package"
      }
    }
    stage('3.CodeQuality'){
      steps{
        sh "echo 'Perfoming CodeQualityAnalysis' "
        sh "mvn sonar:sonar"
        sh "echo 'CodeQualityAnalysis completed' "
      }
    }
    stage('4.uploadNexus'){
      steps{
        sh "mvn deploy"
        sh "echo 'Build and release completed' "
      }
    } 
    stage('5.deploy2UAT'){
      steps{
        sh "echo 'Deployment is ready for the client review' "
        deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://35.153.146.4:8088/')], contextPath: null, war: 'target/*war'
        deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://52.203.0.7:8088/')], contextPath: null, war: 'target/*war'
        deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://54.167.1.91:8088/')], contextPath: null, war: 'target/*war'
      }
    }
    stage('6.ManualApproval'){
      steps{
        sh " echo 'Please review and approve' "
        timeout(time:3, unit:'HOURS')
        {
        input message: 'Application ready for deployment, Please review and approve'
        }
      }
    }
    stage('7.deploy2prod'){
      steps{
        deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://35.153.146.4:8088/')], contextPath: null, war: 'target/*war'
      }
    }
}
post{
  always{
    emailext body: '''Hi Team,

Build status.

Landmark Technologies''', recipientProviders: [buildUser(), developers(), contributor()], subject: 'Build Status', to: 'mbwork1@yahoo.com'
  }
  success{
    emailext body: '''Hi Team,

Build succeded.

Landmark Technologies''', recipientProviders: [buildUser(), developers(), contributor()], subject: 'Build Status', to: 'mbwork1@yahoo.com'
  }
  failure{
    emailext body: '''Hi Team,

Build failed.

Landmark Technologies''', recipientProviders: [buildUser(), developers(), contributor()], subject: 'Build Status', to: 'mbwork1@yahoo.com'
  }
}
}
