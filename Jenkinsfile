pipeline {

  environment {
    registry = "10.100.5.10:5000/justme/myweb"
    dockerImage = ""
    git_cr = 'GIT_CR'
    git_url = 'https://github.com/mannangazi/kubernetes.git'
  }

  agent any

  stages {

       stage('Checkout Source') {
      steps {
        sh 'echo ${BUILD_NUMBER}'
        git credentialsId: ${git_cr} , url: ${git_url}
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "",registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
