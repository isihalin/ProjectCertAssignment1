pipeline {
  environment {
    registry = "diwakarand/projcertimage"
    registryCredential = 'diwakarand'
    dockerImage = ''
  }
  agent {label 'worker01'}
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/isihalin/ProjectCertAssignment1.git'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Run') {
      steps{
        script{
           def container_runner = docker.image(registry + ":$BUILD_NUMBER").run('-d ')
        }
    
      }
    }
    
  
  }
  post { 
        unsuccessful { 
            echo 'Build failed!'
            script{
              container_runner.stop()
            }
        }
    }
}
