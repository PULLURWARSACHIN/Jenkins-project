pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
        INVENTORY_FILE = 'inventory.ini'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/Become-DevOps/proj.git', branch: 'master'
            }
        }

        stage('Install Docker on QA Node') {
            steps {
                sh 'ansible-playbook -i ${INVENTORY_FILE} ansible/install-docker.yml'
            }
        }

        stage('Deploy Code to QA Node') {
            steps {
                sh 'ansible-playbook -i ${INVENTORY_FILE} ansible/deploy-code.yml'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'ansible-playbook -i ${INVENTORY_FILE} ansible/build-image.yml'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'ansible-playbook -i ${INVENTORY_FILE} ansible/run-container.yml'
            }
        }
    }

    post {
        success {
            echo '✅ Deployment completed successfully. Access app at http://<qa-ip>:8080'
        }
        failure {
            echo '❌ Deployment failed. Check logs.'
        }
    }
}
