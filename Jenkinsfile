pipeline {
  agent any
  environment {
        DOCKER_REGISTRY = 'https://index.docker.io/v1/'
        DOCKER_CREDENTIALS_ID = 'Docker-Hub'
    }
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
                script {
            // Login to Docker registry
                    withDockerRegistry([credentialsId: env.Docker-Hub, url: '']) {
                   docker.build('rnallamilli/numeric-app:""BUIL_ID""')
                   docker.withRegistry(env.DOCKER_REGISTRY, env.DOCKER_CREDENTIALS_ID) {
                            docker.image('rnallamilli/numeric-app:""BUIL_ID""').push('latest')
                   }
              }
          
            }
          }
      }
    }
}
