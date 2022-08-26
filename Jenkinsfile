pipeline {
  agent any
  stages {
    stage('fetch') {
      environment {
        tenant_country = sh(script: 'git diff main origin/main --name-only "*.yml"' ,returnStdout: true).trim()
        mutation = sh(script: 'git diff main origin/main --name-only "*.rb"' ,returnStdout: true).trim()
      }
      steps {
        sh 'git fetch --force'
        echo "${env.tenant_country}"
        script {
          datas = readYaml (file: "${env.tenant_country}")
        }

        echo mutation
        echo datas[environment]
        echo sh (script: "rails r ${mutation}", returnStdout: true)
      }
    }

  }
}
