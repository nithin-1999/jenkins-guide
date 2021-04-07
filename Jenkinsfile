pipeline {
  agent {
    node {
      label 'any'
    }

  }
  stages {
    stage('BUILD') {
      post {
        failure {
          echo 'This build has failed. See logs for details.'
        }

      }
      parallel {
        stage('Express Image') {
          steps {
            sh 'docker build -f express-image/Dockerfile             -t nodeapp-dev:trunk .'
          }
        }

      }
    }

    stage('TEST') {
      post {
        success {
          echo 'Build succeeded.'
        }

        unstable {
          echo 'This build returned an unstable status.'
        }

        failure {
          echo 'This build has failed. See logs for details.'
        }

      }
      parallel {
        stage('Mocha Tests') {
          steps {
            sh 'docker run --name nodeapp-dev --network="bridge" -d             -p 9000:9000 nodeapp-dev:trunk'
          }
        }

        stage('Quality Tests') {
          steps {
            sh 'docker login --username nithinc19 --password Nithin@19'
            sh 'docker tag nodeapp-dev:trunk nithinc19/nodeapp-dev:latest'
            sh 'docker push nithinc19/nodeapp-dev:latest'
          }
        }

      }
    }

    stage('DEPLOY') {
      when {
        branch 'main'
      }
      post {
        failure {
          sh 'docker stop nodeapp-dev test-image'
          sh 'docker system prune -f'
          deleteDir()
        }

      }
      steps {
        retry(count: 3) {
          timeout(time: 10, unit: 'MINUTES') {
            sh 'docker tag nodeapp-dev:trunk nithinc19/nodeapp-prod:latest'
            sh 'docker push nithinc19/nodeapp-prod:latest'
            sh 'docker save nithinc19/nodeapp-prod:latest | gzip > nodeapp-prod-golden.tar.gz'
          }

        }

      }
    }

    stage('CLEAN-UP') {
      steps {
        sh 'docker system prune -f'
        deleteDir()
      }
    }

  }
  environment {
    DOCKER = credentials('docker-hub')
  }
}