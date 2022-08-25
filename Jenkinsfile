pipeline {
  agent {
    node {
      label 'variables'
    }

  }
  stages {
    stage('fetch') {
      steps {
        sh 'git fetch --force'
        sh ' {DIFF_YML} = git diff main origin/main --name-only "*.yml"'
      }
    }

    stage('') {
      steps {
        echo '{DIFF_YML}'
      }
    }

  }
  environment {
    DIFF_YML = ' '
  }
}