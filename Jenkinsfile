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
        sh '''#!/bin/bash
git clone --single-branch --branch $GIT_BRANCH $GIT_URL $BUILD_NUMBER
cd $BUILD_NUMBER
sudo mk-build-deps -i -r -t \'apt-get --no-install-recommends -yq\' debian/control
dpkg-buildpackage -b -us -uc -tc'''
      }
    }
    stage('Deploy package') {
      agent {
        node {
          label 'jessie-amd64'
        }

      }
      steps {
        sh '''#!/bin/bash
/var/lib/vyos-build/pkg-build.sh $GIT_BRANCH'''
      }
    }
  }
}