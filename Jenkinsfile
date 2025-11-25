pipeline {
  agent any

  environment {
    GIT_REPO = 'https://github.com/saadkhan024/Ansible-Flask-App.git'
    GIT_BRANCH = 'main'               // <-- change if your branch is different
    INVENTORY = 'inventory.ini'       // <-- change to 'inventory' if that's the file in your repo
    PLAYBOOK  = 'site.yml'            // <-- change if your playbook filename is different
    TARGET    = 'node'                // <-- inventory group or host to limit to, optional
    ANSIBLE_CMD_OPTS = '-vv'
    FLASK_HEALTH = 'http://http://13.234.75.176/health' // <-- change to your Flask app URL/IP
  }

  stages {
    stage('Clone Repo') {
      steps {
        git branch: "${GIT_BRANCH}", url: "${GIT_REPO}"
      }
    }

    stage('Install Dependencies') {
      steps {
        sh '''
          if ! command -v ansible >/dev/null 2>&1; then
            sudo apt-get update -y
            sudo apt-get install -y python3-pip python3-venv
            pip3 install --user ansible
            echo "ansible installed"
          else
            echo "ansible already installed"
          fi
        '''
      }
    }

    stage('Ansible Syntax Check') {
      steps {
        sh "ansible-playbook -i ${INVENTORY} --syntax-check ${PLAYBOOK}"
      }
    }

    stage('Ansible Dry Run (check)') {
      steps {
        sh "ansible-playbook -i ${INVENTORY} --check --diff ${PLAYBOOK} -l ${TARGET} || true"
      }
    }

    stage('Deploy with Ansible') {
      steps {
        // If you add SSH credential id 'ansible-ssh' in Jenkins, uncomment the sshagent block below
        // sshagent (credentials: ['ansible-ssh']) {
          sh "ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i ${INVENTORY} ${PLAYBOOK} ${ANSIBLE_CMD_OPTS} -l ${TARGET}"
        // }
      }
    }

    stage('Post Deploy Health Check') {
      steps {
        sh '''
          sleep 3
          curl -fsS "${FLASK_HEALTH}" || (echo "health check failed" && exit 1)
        '''
      }
    }
  }

  post {
    success {
      echo 'Pipeline finished: SUCCESS'
    }
    failure {
      echo 'Pipeline finished: FAILURE'
    }
  }
}
