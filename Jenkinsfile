/* Deploy option is not deploy to environment */
def deployment = false

/* Build application */
def build_application() {
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

def slackChannel = "#monitoramento"

pipeline {

  agent any

  stages {

    stage('Build Docker Image') {
      /* when {
        expression { deployment }
      } // when */
      steps {
        script {

          build_application()

        } // script

      } // steps

    } // stage Build Docker Image



  } // stages
 






} // pipeline
