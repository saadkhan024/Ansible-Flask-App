pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: ''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                sudo apt update
                sudo apt install -y ansible python3 python3-pip
                '''
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                sh '''
                ansible-playbook -i inventory site.yml
                '''
            }
        }

        stage('Restart Services') {
            steps {
                sh '''
                sudo systemctl restart nginx
                '''
            }
        }
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

