pipeline {
  agent {
    node {
      label 'WORKSTATION'
    }

  }

  tools {
  maven 'workstation-maven'
  jdk 'workstation-jdk8'
}
 
  stages {
    stage('Download the Code') {
      steps {
        git(url: 'http://35.196.214.133/developers/studentapp-code.git', branch: 'master', credentialsId: 'STUDENTAPP-GITLAB-USER')
      }
    }
    stage('Compile Code') {
      steps {
        sh '''echo Hello World
        mvn --version
ls'''
      }
    }
  }
}
