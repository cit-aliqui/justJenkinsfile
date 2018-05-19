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
        sh '''echo Sonaqube Code Analysis is commented.
'''
// mvn sonar:sonar -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=5accbb3a1b534231efcea335678bf3fecc5e0bb0

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
        script {
                IS_NODE_EXISTS = sh (
                  script: 'gcloud compute instances list | grep qanode',
                  returnStatus: true
                )
                if (IS_NODE_EXISTS == 0) {
                  sh "ansible-playbook -i hosts playbooks/qa-deploy.yml --private-key /home/ec2-user/devops.pem -e WAR_LOC=$WORKSPACE"
                } else {
                  sh 'sh playbooks/setup-qa-nodes.sh'
                  sh 'ansible-playbook -i hosts playbooks/set-qa-stack.yml --private-key /home/ec2-user/devops.pem'
                  sh 'ansible-playbook -i hosts playbooks/qa-deploy.yml --private-key /home/ec2-user/devops.pem'
                }
                
        }
      }
    }

    stage('Selenium Testing') {
      steps{
        build 'SELENIUM-TESTING-JOB'
      }
    }

    stage('API Testing') {
      steps{
        sh '''sleep 30
python api-test.py
'''
      }
    }
    stage('Approval') {
      steps {
        input 'Do you want to approve for Stage Deploy?'
      }
    }
  }
}
