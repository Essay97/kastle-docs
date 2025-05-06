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
    stage('test') {
      steps {
        sh 'ls -lah'
      }
    }
  }
}
