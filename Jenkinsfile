pipeline {
  agent any
  environment {
    ANSIBLE_INVENTORY = 'inventory.ini'
    ANSIBLE_PLAYBOOK = 'playbooks/install_nginx.yml'
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Run Ansible') {
      steps {
        sshagent (credentials: ['ec2_key']) {
          sh '''
            echo "Running ansible-playbook ..."
            which ansible || echo "ansible not found on agent"
            ansible-playbook -i ${ANSIBLE_INVENTORY} ${ANSIBLE_PLAYBOOK} -vv
          '''
        }
      }
    }
  }
  post {
    success { echo 'Playbook applied successfully' }
    failure { echo 'Playbook failed - check console output' }
  }
}

