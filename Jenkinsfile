pipeline {
  agent {
    node {
      label 'test'
    }

  }
  stages {
    stage('test') {
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