pipeline {
  agent any
  stages {
    stage('fetch') {
      environment {
        tenant_country = sh(script: 'git diff main origin/main --name-only "*.yml"' ,returnStdout: true).trim()
      }
      steps {
        sh 'git fetch --force'
        echo "${tenant_country}"
      }
    }

  }
}