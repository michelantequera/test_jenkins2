pipeline {
  agent any
  stages {
    stage('fetch') {
       environment {
        tenant_country = sh(script: 'git diff main origin/main --name-only "*.yml"' ,returnStdout: true).trim()
      }
      steps {
        sh 'git fetch --force'
        echo "${env.tenant_country}"
        echo sh("nano ${env.tenant_country}")
      }
    }
  }
}
