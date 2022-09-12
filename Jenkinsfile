pipeline {
  agent { label 'small'}
  stages {
    stage('fetch') {
      environment {
        SLACK_CHANNEL = "prueba-notificaciones"
      }
      steps {
        script {
        pipelineThread = slackPipelineStart("$SLACK_CHANNEL")
        SetEnvMsg = slackStageStart(thread: pipelineThread)
      }
        sh 'git checkout main'
        sh 'git checkout origin/feature/R2D2-8-ccreacion-jenkinsfile'
        sh 'git fetch --force'
        script {
          tenant_country = sh(script: 'git diff main origin/feature/R2D2-8-ccreacion-jenkinsfile --name-only "*.yml"' ,returnStdout: true).trim()
          mutation = sh(script: 'git diff main origin/main --name-only "*.rb"' ,returnStdout: true).trim()
          datas = readYaml (file: "${env.tenant_country}")
        }
        echo sh (script: "rails r ${mutation}", returnStdout: true)
      }
      post {
      failure {
        slackStageUpdate(thread: SetEnvMsg, color: 'danger')
      }
      success {
        slackStageUpdate(color: 'good', thread: SetEnvMsg)
      }
    }
    }
  }
}
