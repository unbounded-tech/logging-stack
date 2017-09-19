pipeline {
  agent {
    label "docker"
  }
  stages {
    stage("deploy") {
      when {
        branch "master"
      }
      steps {
        sh "docker stack deploy -c logging.yml logging"
      }
    }
  }
  post {
    always {
      sh "docker system prune -f"
    }
    failure {
      slackSend(
        color: "danger",
        message: "${env.JOB_NAME} failed: ${env.RUN_DISPLAY_URL}"
      )
    }
    success {
      slackSend(
        color: "success",
        message: "${env.JOB_NAME} succeeded: ${env.RUN_DISPLAY_URL}"
      )
    }
  }
}