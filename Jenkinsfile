pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          serviceAccountName: deploy
          containers:
          - name: kubectl
            image: alpine/k8s:1.20.7
            command:
            - cat
            tty: true
        '''
    }
  }
  environment {
    ENVIRONMENTS = ['prod', 'sandbox', 'int']
  }
  stages {
    stage('Validate') {
      when {
        changeRequest()
      }
      steps {
        container('kubectl') {
          script {
            for (environment in env.ENVIRONMENTS) {
              stage("Validating ${environment}") {
                sh "kustomize build ${environment}"
              }
            }
          }
        }
      }
    }
    stage('Deploy int') {
      when {
        branch "main"
      }
      steps {
        container('kubectl') {
          sh "kubectl apply -k int"
        }
      }
      post {
        success {
          echo 'I succeeded INT deploy!'
        }
        failure {
          echo 'I failed INT  deploy!!'
        }
      }
    }
    stage('Deploy sandbox') {
      when {
        branch "main"
      }
      steps {
        container('kubectl') {

          sh "kubectl apply -k sandbox"
        }
      }
      post {
        success {
          echo 'I succeeded SANDBOX deploy!'
        }
        failure {
          echo 'I failed SANDBOX deploy!!'
        }
      }
    }
    stage('Deploy production') {
      when {
        branch "main"
      }
      input {
        message "Deploy production?"
        ok "Yes"
      }
      steps {
        container('kubectl') {
          sh "kubectl apply -k prod"
        }
      }
      post {
        success {
          echo 'I succeeded PROD deploy!'
        }
        failure {
          echo 'I failed PROD deploy!'
        }
      }
    }
  }
}
