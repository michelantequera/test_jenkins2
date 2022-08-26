pipeline {
  agent any
  stages {
    stage('fetch') {
      steps {
        def yml = sh (script: 'git diff main origin/main --name-only "*.yml"', returnStdout: true)
        sh 'git fetch --force'
      }
    }
    stage('error') {
      steps {
        echo yml
      }
    }
  }
}
