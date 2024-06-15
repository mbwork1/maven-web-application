pipeline{
  agent any 
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
        sh "echo 'testing must passed to create artifacts ' "
        sh "mvn clean package"
      }
    }
    stage('3.CodeQuality'){
      steps{
        sh "echo 'Perfoming CodeQualityAnalysis' "
        sh "mvn sonar:sonar"
      }
    }
    stage('4.uploadNexus'){
      steps{
        sh "mvn deploy"
      }
    } 
    stage('5.deploy2prod'){
      steps{
        deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://35.153.146.4:8088/')], contextPath: null, war: 'target/*war'
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
}
