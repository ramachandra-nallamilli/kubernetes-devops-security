pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }
        }   
	stage('Unit Tests') {
            steps {
              sh "mvn test"
            }
            post {
              always{
                junit "target/surefire-reports/*.xml"
                jacoco execPattern: 'target/jacoco.exec'
              }
            }
        }
  stage('Docker image Build and Push') {
          steps {
            docker.withRegistry(url: "", credentialsId: "Docker-Hub"){
            sh 'printenv'
            sh 'docker build -t rnallamilli/numeric-app:""BUIL_ID""'
            sh 'docker push rnallamilli/numeric-app:""BUIL_ID""'
            }
          }
      }
    }
}
