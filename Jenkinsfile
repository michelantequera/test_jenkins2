pipeline {
  agent any
  stages {
    stage('fetch') {
      steps {
        sh 'git diff main origin/main --name-only "*.yml"'
        sh 'git fetch --force'
      }
    }

    stage('error') {
      steps {
        echo 'DIFF_YML'
      }
    }

  }
}
