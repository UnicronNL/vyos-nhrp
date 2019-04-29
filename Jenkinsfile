pipeline {
  agent none
  stages {
    stage('build-package') {
      agent {
        node {
          label 'jessie-amd64'
        }

      }
      steps {
        git(url: '$GIT_URL', branch: '$GIT_BRANCH', poll: true)
      }
    }
    stage('Deploy package') {
      steps {
        sh '''#!/bin/bash
/var/lib/vyos-build/pkg-build.sh $GIT_BRANCH'''
      }
    }
  }
}