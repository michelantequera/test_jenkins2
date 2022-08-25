pipeline {
  agent any
  stages {
    stage('fetch') {
      steps {
        sh 'git fetch --force'
        sh 'git diff \'*.yml\' main origin/main'
      }
    }

  }
}