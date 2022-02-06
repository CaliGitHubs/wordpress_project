pipeline {

  environment {
    dockerimagename = "calidockers/wordpress"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url: 'https://github.com/CaliGitHubs/wordpress_project.git',
        branch: 'main'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    /*stage('Creating a secret') {
      steps {
        script {
         //kubernetesDeploy(configs: "wordpress.yml", kubeconfigId: "kubernetes")
          kubectl create secret generic mysql-pass --from-literal=password=test1
        }
      }
    }
    */
    stage('Deploying App to Kubernetes') {
      steps {
        script {
         //kubernetesDeploy(configs: "wordpress.yml", kubeconfigId: "kubernetes")
          kubectl create -f wordpress.yml
        }
      }
    }
  }

}
