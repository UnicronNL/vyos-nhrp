pipeline {
  agent {
    docker {
      image 'higebu/vyos-build:current'
      label 'jessie-amd64'
      args '--privileged --sysctl net.ipv6.conf.lo.disable_ipv6=0 -v /home/jenkins/.ssh:/home/vyos_bld/.ssh -e GOSU_UID=1006 -e GOSU_GID=1006'
    }
  }
  stages {
    stage('build-package') {
      steps {
        sh '''#!/bin/bash
git clone --single-branch --branch $GIT_BRANCH $GIT_URL $BUILD_NUMBER
cd $BUILD_NUMBER
sudo mk-build-deps -i -r -t \'apt-get --no-install-recommends -yq\' debian/control
dpkg-buildpackage -b -us -uc -tc'''
      }
    }
    stage('Deploy package') {
      steps {
        sh '''#!/bin/bash
echo $HOME
ls -al $HOME/.ssh
cd $BUILD_NUMBER
/usr/local/bin/pkg-build.sh $GIT_BRANCH'''
      }
    }
  }
}
