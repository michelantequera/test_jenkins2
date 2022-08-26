pipeline {
  agent any
  stages {
    stage('fetch') {
      steps {
        RESULT_DIFF = sh (script: 'git diff main origin/main --name-only "*.yml"', returnStdout: true)
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
