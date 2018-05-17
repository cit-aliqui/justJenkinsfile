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
        sh '''mvn clean compile
'''
      }
    }
    stage('Static Code Analysis') {
      steps {
        sh '''mvn sonar:sonar -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=5accbb3a1b534231efcea335678bf3fecc5e0bb0
'''
      }
    }
    stage('Packaging') {
      steps {
        sh '''mvn package
'''
      }
    }

    stage('Setup QA Nodes'){
      steps {
        sh '''sh playbooks/setup-qa-nodes.sh
'''
        sh '''ansible-playbook -i hosts playbooks/set-qa-stack.yml --private-key /home/ec2-user/devops.pem 
'''
      }
    }
  }
}
