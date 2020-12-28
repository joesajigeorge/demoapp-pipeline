pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Build demo app'
        sh 'sh run_build_script.sh'
      }
    }

    stage('Tests') {
      parallel {
        stage('Linux Tests') {
          steps {
            echo 'Run Linux Tests'
            sh 'sh run_linux_tests.sh'
          }
        }

        stage('Windows Tests') {
          steps {
            echo 'Run Windows Tests'
          }
        }

      }
    }

    stage('Deploy to staging') {
      steps {
        echo 'deploy to staging'
        input 'Ok to deploy to production?'
      }
    }

    stage('Deploy to prod') {
      steps {
        echo 'Deploy to production'
      }
    }

  }
  post {
    always {
      echo 'One way or another, I have finished'
      archiveArtifacts(artifacts: 'target/demoapp.jar', fingerprint: true)
    }

    success {
      echo 'I succeeded!'
    }

    failure {
      mail(to: 'sajigeorge.joe@gmail.com', subject: "Failed Pipeline ${currentBuild.fullDisplayName}", body: " For details about the failure, see ${env.BUILD_URL}")
    }

  }
}