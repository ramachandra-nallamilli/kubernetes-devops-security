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
            sh 'printenv'
            sh 'docker build -t rnallamilli/numeric-app:""GIT_COMMIT""'
            sh 'docker push rnallamilli/numeric-app:""GIT_COMMIT""'
          }
      }
    }
}
