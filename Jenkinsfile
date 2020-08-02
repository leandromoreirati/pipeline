/* Deploy option is not deploy to environment */
def deployment = true

/* Build application */
def build_docker_image() {
  sh'''
    docker run hello-world
  '''
}

/* Return branh type based on branch name */
def get_branch(String branch_name) {
  if (branch_name =~ '.*master')
    return "master"
  else
    Error: "Branch ${branch_name} not found."
}

slackChannel = "#monitoramento"

pipeline {

  agent any

  stages {

    stage('Build Docker Image') {

      when {
        expression { deployment }
      } // when

      steps {
        script {

          build_docker_image()

        } // script

      } // steps

    } // stage Build Docker Image

    stage('Deployment') {

      when {
        expression { deployment }
      } // when

      steps {
        script {

          switch(get_branch("${env.BRANCH_NAME}")) {
            case "master":
              env.ENV = "production"
              break
          } // switch

          slackSend(
              channel: slackChannel,
              message: "Job ${env.BUILD_NUMBER}*: Starting *${env.ENV}* deployment with code from *${env.BRANCH_NAME}* - commit hash *${env.GIT_COMMIT}*\n *More info: ${env.JOB_URL}*",
              color: "warning"
          )

          try {

            build_docker_image()

            slackSend(
                channel: slackChannel,
                message: "Job ${env.BUILD_NUMBER}*: *${env.ENV}* deployment *SUCESS* *\n *More info: ${env.JOB_URL}*",
                color: "good"
            )

          } catch(exc) {
              currentBuild.result = 'FAILURE'
              slackSend(
                  channel: slackChannel,
                  message: "Job ${env.BUILD_NUMBER}*: *${env.ENV}* deployment *FAILURE* *\n *More info: ${env.JOB_URL}*",
                  color: "danger"
              )

          } // catch

        } // script

      } // steps

    } // stage Build Docker Image



  } // stages
 






} // pipeline
