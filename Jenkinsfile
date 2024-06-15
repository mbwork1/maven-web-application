pipeline{
  agent any 
  tools {
    maven "Maven 3.9.7"
  }  
  stages {
    stage('1GetCode'){
      steps{
        sh "echo 'cloning the latest application version' "
        git branch: 'master', url: 'https://github.com/mbwork1/maven-web-application.git'
      }
    }
    stage('3Test+Build'){
      steps{
        sh "echo 'running JUnit-test-cases' "
        sh "echo 'testing must passed to create artifacts ' "
        sh "mvn clean package"
      }
    }
    stage('4CodeQuality'){
      steps{
        sh "echo 'Perfoming CodeQualityAnalysis' "
        sh "mvn sonar:sonar"
      }
    }
    stage('5uploadNexus'){
      steps{
        sh "mvn deploy"
      }
    } 
    stage('8deploy2prod'){
      steps{
        deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://54.87.44.152:8088/')], contextPath: null, war: 'target/*war'
      }
    }
}
}
