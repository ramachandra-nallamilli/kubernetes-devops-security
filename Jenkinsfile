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
      stage('Build and Push Image') {
          steps {
              script {
                  //def buildId = env.BUILD_ID.replaceAll("[^a-zA-Z0-9_.-]", "-")
                  def imageName = "rnallamilli/numeric-app:${BUILD_ID}"
                  // Login to Docker registry
                  withDockerRegistry([credentialsId: env.DOCKER_CREDENTIALS_ID, url: env.DOCKER_REGISTRY]) {
                      // Build Docker image
                      docker.build(imageName)

                      // Push Docker image
                      docker.withRegistry(env.DOCKER_REGISTRY, env.DOCKER_CREDENTIALS_ID) {
                          docker.image(imageName).push('latest')
                      }
                  }
              }
          }
    }
    stage('Kubernetes Deployment - Dev'){
        steps{
            withKubeConfig([credentialsId: 'kubeconfig']) {
              sh "sed -i 's#replace#rnallamilli/numeric-app:${BUILD_ID}#g' k8s_deployment_service.yaml"
              sh 'kubectl apply -f k8s_deployment_service.yaml'
            }
        }        
    }
  }
}
