/* Deploy option is not deploy to environment */
def deployment = false

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

/* Slack notifiction */
def notifySlack(String buildStatus = 'STARTED') {
    // Build status of null means success.
    buildStatus = buildStatus ?: 'SUCCESS'

    def color

    if (buildStatus == 'STARTED') {
        color = '#D4DADF'
    } else if (buildStatus == 'SUCCESS') {
        color = '#BDFFC3'
    } else if (buildStatus == 'UNSTABLE') {
        color = '#FFFE89'
    } else {
        color = '#FF9FA1'
    }

    def msg = "${buildStatus}: `${env.JOB_NAME}` #${env.BUILD_NUMBER}:\n${env.BUILD_URL}"

    slackSend(color: color, message: msg)
}

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

          notifySlack()

        } // script

      } // steps

    } // stage Build Docker Image

/*     stage('Deployment') {

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
          } // catch

        } // script

      } // steps

    } // stage Build Docker Image
 */


  } // stages
 






} // pipeline
