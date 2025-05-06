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

    stage('Backup') {
      steps {
        // Backup current site
        sh '''
          SITE_DIR="/var/www/kastle-docs/html"
          BACKUP_DIR="/var/www/kastle-docs/backups"
          TODAY=$(date +%Y%m%d)
          BACKUP_FILE="${BACKUP_DIR}/backup-${TODAY}.zip"

          sudo zip -r "$BACKUP_FILE" "$SITE_DIR"
        '''
      }
    }

    stage('Deploy') {
      steps {
        sh 'sudo cp -r site /var/www/kastle-docs/html'
      }
    }

    stage('Restart Nginx') {
      steps {
        sh 'sudo /bin/systemctl restart nginx'
      }
    }

    stage('Cleanup') {
      steps {
        cleanWs cleanWhenAborted: false, cleanWhenFailure: false, cleanWhenNotBuilt: false, cleanWhenUnstable: false
      }
    }
  }
}
