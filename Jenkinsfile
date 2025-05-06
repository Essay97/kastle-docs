pipeline {
  agent any
  
  parameters {
    gitParameter type: 'PT_TAG',
                 name: 'Tag',
                 defaultValue: 'origin/main',
                 selectedValue: 'NONE',
                 sortMode: 'DESCENDING_SMART',
                 tagFilter: '*'
  }

  stages {
    stage('Build') {
      steps {
        sh """
          docker run --rm -v "${env.WORKSPACE}:/docs" squidfunk/mkdocs-material:9.6 build
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
