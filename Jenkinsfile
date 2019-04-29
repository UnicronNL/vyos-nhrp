pipeline {
  agent {
      docker {
          image 'higebu/vyos-build:$GIT_BRANCH'
          label 'jessie-amd64'
          args '--privileged --sysctl net.ipv6.conf.lo.disable_ipv6=0 -e GOSU_UID=1006 -e GOSU_GID=1006'
      }
  }
  stages {
    stage('build-package') {
      steps {
        sh '''#!/bin/bash
rm -f ../*.deb
sudo mk-build-deps -i -r -t \'apt-get --no-install-recommends -yq\' debian/control

dpkg-buildpackage -b -us -uc -tc
'''
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
