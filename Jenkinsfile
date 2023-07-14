pipeline {
  agent any
  environment {
    GITHUB_REPO="https://github.com/lyhan12/hello_world_server"
    DOCKER_REPO="lyhan12/hello_world_server"
    VERSION=1.0
  }
  stages {
    stage("CheckOut") {
      steps {
        git url: "$GITHUB_REPO"
            branch: "main"
      }
    }
    stage("Code Build") {
      steps {
        sh "echo 'Code Build'"
      }
    }
    stage("Unit Test") {
      steps {
        sh "echo 'Unit Test'"
      }
    }
    stage("Docker Build") {
      steps {
        sh "docker build -t $DOCKER_REPO:$VERSION ."
      }
    }
    stage("Docker Push") {
      steps {
        sh "echo 'Docker Push'"
      }
    }
    stage("Deploy") {
      steps {
        sh "echo 'Deploy'"
      }
    }
  }
  post {
    success {
      slackSend (
          channel: "#랜덤",
          color: "#00FF00",
          message: "SUCCESS : Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]"
          )
    }
    failure {
      slackSend (
          channel: "#랜덤",
          color: "#FF0000",
          message: "FAIL : Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]"
          )
    }
  }
}
