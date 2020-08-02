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

    stage('Notify Build Start') {

      when {
        expression { deployment }
      } // when

      steps {
        script {

          slackSend(
              channel: slackChannel.
              message: "Job ${env.BUILD_NUMBER}*: Starting *${env.ENV}* deployment with code from *${env.BRANC_NAME}* - commit hash *${env.GIT_COMMIT}*\n *More info: ${env.JOB_URL}*",
              color: "warning"

          )

        } // script

      } // steps

    } // stage Build Docker Image

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

          notifySlack()

          try {

            build_docker_image()

            notifySlack()
          } catch(exc) {
              currentBuild.result = 'FAILURE'
              build_docker_image()
              notifySlack()
          } finally {
              notifySlack(currentBuild.result)
          } // finally

        } // script

      } // steps

    } // stage Build Docker Image



  } // stages
 






} // pipeline
