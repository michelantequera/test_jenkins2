pipeline {
  agent any
  stages {
    stage('fetch') {
      steps {
         env.tenant_country = sh (script: 'git diff main origin/main --name-only "*.yml"',returnStdout: true).trim()
        sh 'git fetch --force'
      }
    }

    stage('error') {
      steps {
        echo env.tenant_country
      }
    }
  }
}
