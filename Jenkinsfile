pipeline{
   agent {
    docker {
      image 'maven:3.6.3-jdk-11'
      args '-v /root/.m2:/root/.m2'
    }
  }
  stages {
     stage('MavenVertion'){
      steps{
        echo 'Maven Vaersion'
        sh 'mvn -version'
       
      }
     }
    stage('Clean'){
      steps{
        echo 'Clean'
        sh 'mvn clean'
       
      }
     }
    stage('Compile'){
      steps{
        echo 'Compile'
        sh 'mvn compile'
        
      }
     }
    stage('package'){
      steps{
        echo 'Packing '
        sh 'mvn -B -DskipTests clean package'
        
      }
     }
    stage('Test'){
      steps{
        echo 'Maven test'
        sh 'mvn test'
      }
      
  }
     stage("Build & SonarQube analysis") {
            agent any
            steps {
              withSonarQubeEnv('SonarSpring') {
                sh 'java -version'
                sh 'mvn clean package sonar:sonar'
              }
            }
          }
     stage('Deploy to artifactory'){
        steps{
        rtUpload(
         serverId : 'ARTIFACTORY_SERVER',
         spec :'''{
           "files" :[
           {
           "pattern":"target/*.jar",
           "target":"art-doc-dev-loc"
           }
           ]
         }''',
         
      )
      }
     }
    }
  }
