pipeline {
  agent any
  stages {
    stage('fetch') {
      steps {
        sh 'git fetch --force'
        sh 'git diff main origin/main --name-only "*.yml"'
      }
    }

  }
}