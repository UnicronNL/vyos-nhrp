pipeline {
  agent none
  stages {
    stage('build-package') {
      agent {
        docker {
          image 'higebu/vyos-build:current'
          label 'jessie-amd64'
          args '--privileged --sysctl net.ipv6.conf.lo.disable_ipv6=0'
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
pwd
ls -al
cd $BUILD_NUMBER
pwd
ls -al
# /var/lib/vyos-build/pkg-build.sh $GIT_BRANCH'''
      }
    }
  }
}
