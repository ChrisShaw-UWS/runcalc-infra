pipeline {
    agent any

    stages {

        stage('Checkout Infra Repo') {
            steps {
                git 'https://github.com/ChrisShaw-UWS/runcalc-infra.git'
            }
        }

        stage('Run Ansible Deployment') {
            steps {
                echo "Deploying using Ansible..."

                sh '''
                ansible-playbook ansible/deploy.yml -i ansible/inventory.ini
                '''
            }
        }

        stage('Health Check') {
            steps {
                echo "Checking application health..."

                sh '''
                sleep 10
                curl -f http://54.88.196.120 || exit 1
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful via Ansible! (Thank goodness)"
        }
        failure {
            echo "❌ Deployment failed via Ansible. Check logs for details (again)."
        }
    }
}