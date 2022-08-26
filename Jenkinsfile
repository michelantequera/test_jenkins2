pipeline {
  agent any
  stages {
    stage('fetch') {
      steps {
        GIT_COMMIT_EMAIL = sh (script: 'git diff main origin/main --name-only "*.yml"',returnStdout: true).trim()
        sh 'git fetch --force'
      }
    }

    stage('error') {
      steps {
        echo GIT_COMMIT_EMAIL
      }
    }

  }
}
