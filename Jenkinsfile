pipeline {
  agent any

  options {
    disableConcurrentBuilds()
    timestamps()
    buildDiscarder(logRotator(numToKeepStr: '20'))
  }

  triggers {
    cron('H 4 * * *')
  }

  environment {
    ADMIN_PASSWORD = credentials('PROJECTZOMBOID_ADMIN_PASSWORD')
    HOST_USER = credentials('PROJECTZOMBOID_HOST_USER')
    RCON_PASSWORD = credentials('PROJECTZOMBOID_RCON_PASSWORD')
    SERVER_PASSWORD = credentials('PROJECTZOMBOID_SERVER_PASSWORD')
  }

  stages {
    stage('Deploy') {
      steps {
        sh 'docker compose -f docker-compose-project-zomboid-server.yml pull'
        sh 'docker compose -f docker-compose-project-zomboid-server.yml up -d --force-recreate'
      }
    }
  }

  post {
    always {
      sh 'docker image prune -f'
    }
  }
}
