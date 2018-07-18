pipeline {
  agent {
    docker {
      image 'node:8-alpine'
      args '-u root:node'
    }
  }

  environment {
    PATH = '~/.local/bin:${PATH}'
  }

  stages {
    stage('Prepare') {
      steps {
        sh 'apk update'
        sh 'apk add --update python py-pip'
        sh 'apk add --update build-base'
      }
    }
    stage('Prepare - pre-production') {
      when {
        changeRequest target: 'production'
      }
      steps {
        sh 'pip install awscli --upgrade --user'
      }
    }
    stage('Prepare - production') {
      when {
        branch 'production'
      }
      steps {
        sh 'npm install -g gh-pages'
      }
    }
    stage('Build') {
      steps {
        sh 'npm install'
        sh 'npm run build'
      }
    }
    stage('Deploy - pre-production') {
      when {
        changeRequest target: 'production'
      }
      steps {
        sh 'aws s3 sync --delete --acl public-read public s3://centreon-labs/${BRANCH_NAME}'
      }
    }
    stage('Deploy - production') {
      when {
        branch 'production'
      }
      steps {
        sh 'echo MEP'
      }
    }
  }
}
