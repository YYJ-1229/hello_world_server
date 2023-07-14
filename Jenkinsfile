pipeline {
  agent any
  environment {
     DOCKERHUB = credentials("dockerhub")
    GITHUB_REPO="https://github.com/YYJ-1229/hello_world_server"
    DOCKER_REPO="yangdori/hello_world_server"
    VERSION=1.0
  }
  stages {
    stage("CheckOut") {
      steps {
        git url: "$GITHUB_REPO",
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
        sh "docker login -u $DOCKERHUB_USR -p $DOCKERHUB_PSW"
        sh "docker push $DOCKER_REPO:$VERSION"
        sh "docker logout"
      }
    }
    stage("Deploy") {
      steps([$class: 'BapSshPromotionPublisherPlugin']) {
        sshPublisher(
          continueOnError: false, failOnError: true,
          publishers: [
            sshPublisherDesc(
              configName: "remote_server",
              verbose: true,
              transfers: [
                sshTransfer(execCommand: "touch test_file")
              ]
            )
          ]
        )
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
