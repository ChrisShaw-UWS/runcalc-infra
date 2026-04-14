pipeline {
    agent any

    parameters {
        string(name: 'IMAGE_TAG', defaultValue: 'v1.0.8', description: 'Docker Image Version')
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/ChrisShaw-UWS/runcalc-infra.git'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh """
                ansible-playbook -i inventory.ini deploy.yml \
                --extra-vars "image_tag=${IMAGE_TAG}"
                """
            }
        }

        stage('Verify App') {
            steps {
                sh """
                curl http://LATEST_EC2_PUBLIC_IP
                """
            }
        }
    }
}