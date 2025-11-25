pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Ansible (if needed)') {
            steps {
                sh '''
                if ! command -v ansible >/dev/null 2>&1; then
                  sudo apt update
                  sudo apt install -y ansible python3-pip
                fi
                '''
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh '''
                ansible-playbook -i inventory.ini site.yml
                '''
            }
        }
    }
}

