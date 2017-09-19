pipeline {
  agent {
    label "docker"
  }
  stages {
    stage("setup") {
      when {
        branch "master"
      }
      steps {
        slackSend(
          color: "info",
          message: "${env.JOB_NAME} started: ${env.RUN_DISPLAY_URL}"
        )
        try {
          sh "docker network create --driver overlay logging"
        } catch (e) {
          // network already exists, nothing to see here, move along
        }
      }
    }
    stage("deploy") {
      when {
        branch "master"
      }
      environment {
        LOGGING_DOMAIN = logging.imakethingsfortheinternet.com
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