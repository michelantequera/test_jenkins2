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
        def datas = readYaml file: env.tenant_country
        echo sh("cat ${datas}")
      }
    }
  }
}
