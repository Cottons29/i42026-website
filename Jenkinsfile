pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'composer install'
                sh 'cp .env.example .env'
                sh 'php artisan key:generate'
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Deploy') {
            steps {
                sh 'ansible-playbook -i deploy/inventory.ini deploy/deploy.yml'
            }
        }
    }
    post {
        success {
            mail to: 'carteria.noreply@gmail.com',
                 subject: "Hello From Jenkins",
                 body: "Hello From Jenkins"
        }
        failure {
            mail to: 'carteria.noreply@gmail.com',
                 subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                 body: "Something is wrong with ${env.BUILD_URL}"
        }
    }
}
