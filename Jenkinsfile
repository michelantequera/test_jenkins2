import groovy.transform.Field

@Field def author = ''
@Field def message = ''
@Field def mutation = ''
@Field def namespace = ''
@Field def cluster = ''
@Field def file_path = ''
@Field def tenant = ''
@Field def pod = 'buk-worker'
@Field def configs = [
  Chile: [
    cluster: 'chile-enterprise-eks',
    namespace: 'enterprise-chile'
  ],
  Colombia: [
    cluster: 'enterprise-colombia',
    namespace: 'enterprise-colombia',
  ],
  Mexico: [
    cluster: 'enterprise-mexico-eks',
    namespace: 'enterprise-mexico',
  ],
  Peru: [
    cluster: 'enterprise-peru',
    namespace: 'enterprise-peru'
  ]
]

pipeline {
  agent { label 'rapanui'}
  environment {
    SLACK_CHANNEL = "#rapanui"
    DOCKER_IMAGE = "770092832210.dkr.ecr.sa-east-1.amazonaws.com/buk/sre-tools:latest"
  }
  stages {
    stage('repo'){
      steps {
        sshagent(["bermuditas-ssh"]) {
          sh 'git checkout main'
          sh 'git pull --force'
          script {
            yml_file = sh(script: 'git diff-tree -r --no-commit-id HEAD HEAD~1 --name-only "*.yml"' ,returnStdout: true).trim()
            file_path = sh(script: 'git diff-tree -r --no-commit-id HEAD HEAD~1 --name-only "*.rb"' ,returnStdout: true).trim()
            message = sh(returnStdout: true, script: 'git log -1 --pretty=%B HEAD').trim()
            author = sh(returnStdout: true, script: "git show -s --format='%ae' HEAD").trim()
            mutation = sh(script: """cat ${file_path}""" ,returnStdout: true).trim()
            config_input = readYaml (file: yml_file) 
            tenant = config_input['tenant']
            country_config = configs[config_input['country']]
            namespace = country_config.namespace
            cluster = country_config.cluster
          }
        }
      }
    }
    stage('agente') {
      agent {
         docker {
           image "$DOCKER_IMAGE"
           args "-u root"
           reuseNode true
         }
       }
      steps {
        sh """
         aws eks update-kubeconfig --name $cluster --region sa-east-1
         kubectl config set-context --current --namespace=$namespace
         kubectl exec -i -c $pod \$(kubectl get pod -l "app.kubernetes.io/name=$pod" -o jsonpath='{.items[0].metadata.name}') -- env TENANT=${tenant} bin/rails r "${mutation}"
         """
      }
      post {
        failure {
          slackSend color: "danger", message: "Rapanui con errores por $author commit: $message", channel: "$SLACK_CHANNEL"
        }
        success {
          slackSend color: "good", message: "Rapanui ejecutado por $author commit: $message", channel: "$SLACK_CHANNEL"
        }
      }
    }
  }
}
