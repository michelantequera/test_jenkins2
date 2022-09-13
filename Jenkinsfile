import groovy.transform.Field

@Field def SetEnvMsg = ''

pipeline {
  agent { label 'rapanui'}
  environment {
    SLACK_CHANNEL = "prueba-notificaciones"
    cluster = "staging-eks"
    namespace = "staging-chile"
    pod = "buk-worker"
    DOCKER_IMAGE = "770092832210.dkr.ecr.sa-east-1.amazonaws.com/buk/sre-tools:latest"
  }
  stages {
    stage('fetch') {
      agent {
         docker {
           image "$DOCKER_IMAGE"
           reuseNode true
         }
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
        sh """
         env | sort
         aws sts get-caller-identity
         aws eks update-kubeconfig --name staging-eks --region sa-east-1 --role-arn arn:aws:iam::770092832210:role/jenkins-agent-eks-rapanui
         kubectl config set-context --current --namespace=$NAMESPACE
         kubectl exec -it -c $pod -t \$(kubectl get pod -l "app.kubernetes.io/name=$pod" -o jsonpath='{.items[0].metadata.name}') -- bin/rails r print 'hola'
         """
        echo datas['cluster']
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
