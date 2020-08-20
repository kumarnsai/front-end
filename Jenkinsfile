pipeline {
  agent none
  stages {
    stage('Build') {
      agent {
        docker {
          image 'schoolofdevops/node:4-alpine'
        }

      }
      steps {
        echo 'Building..'
        sh 'npm install'
      }
    }

    stage('Test') {
      agent {
        docker {
          image 'schoolofdevops/node:4-alpine'
        }

      }
      steps {
        echo 'Testing'
        sh '''npm install
npm test'''
      }
    }

    stage('Packaging') {
      agent {
        docker {
          image 'schoolofdevops/node:4-alpine'
        }

      }
      steps {
        echo 'Packaging....'
        sh '''npm install
npm run package'''
        archiveArtifacts '**/distribution/*.zip'
      }
    }

    stage('Docker Build and pacakge') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
            def dockerImage = docker.build("kumar123/frontend:v${env.BUILD_ID}", "./")
            dockerImage.push()
            dockerImage.push("latest")
          }
        }

      }
    }

  }
  post {
    always {
      echo 'this pipeline has completed'
    }

  }
}