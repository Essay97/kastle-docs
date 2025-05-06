pipeline {
  agent any
  
  parameters {
    gitParameter type: 'PT_BRANCH_TAG',
                 name: 'TAG_OR_BRANCH',
                 defaultValue: 'origin/main',
                 selectedValue: 'NONE',
                 sortMode: 'DESCENDING_SMART',
                 tagFilter: '*'
  }

  environment {
    JENKINS_UID = '110'
    JENKINS_GID = '118'
  }

  stages {
    stage('Build') {

      steps {
        sh """
          docker run --rm -u ${JENKINS_UID}:${JENKINS_GID} -v "${env.WORKSPACE}:/docs" squidfunk/mkdocs-material:9.6 build
        """
      }
    }

    stage('Deploy') {
      steps {
        sh 'ls -lah'
      }
    }
  }
}
