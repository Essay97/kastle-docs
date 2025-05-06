pipeline {
  agent any
  
  parameters {
    gitParameter branch: '', branchFilter: '.*', defaultValue: 'origin/master', name: 'Tag', quickFilterEnabled: false, selectedValue: 'NONE', sortMode: 'NONE', tagFilter: '*', type: 'GitParameterDefinition'
  }

  stages {
    stage('test') {
      steps {
        sh 'll'
      }
    }
  }
}
