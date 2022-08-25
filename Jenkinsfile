pipeline {
  agent {
    node {
      label 'test'
    }

  }
  stages {
    stage('fetch') {
      steps {
        sh 'git fetch --force'
      }
    }

    stage('read yml') {
      steps {
        readTrusted 'config.yml'
      }
    }

  }
  environment {
    AMBIENTE = ''
  }
}