pipeline {
    agent any

    stages {
        stage('Notify Start') {
            steps {
                mail to: 'lycuttons@gmail.com',
                     subject: "Build Started: ${currentBuild.fullDisplayName}",
                     body: "Build ${currentBuild.fullDisplayName} has started.\n\nCheck: ${env.BUILD_URL}"
            }
        }
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
            mail to: 'lycuttons@gmail.com',
                 subject: "Build Success: ${currentBuild.fullDisplayName}",
                 body: "Build ${currentBuild.fullDisplayName} completed successfully.\n\nCheck: ${env.BUILD_URL}"
        }
        failure {
            mail to: 'lycuttons@gmail.com',
                 subject: "Build Failed: ${currentBuild.fullDisplayName}",
                 body: "Build ${currentBuild.fullDisplayName} has failed.\n\nCheck: ${env.BUILD_URL}"
        }
        aborted {
            mail to: 'lycuttons@gmail.com',
                 subject: "Build Aborted: ${currentBuild.fullDisplayName}",
                 body: "Build ${currentBuild.fullDisplayName} was aborted.\n\nCheck: ${env.BUILD_URL}"
        }
        unstable {
            mail to: 'lycuttons@gmail.com',
                 subject: "Build Unstable: ${currentBuild.fullDisplayName}",
                 body: "Build ${currentBuild.fullDisplayName} is unstable.\n\nCheck: ${env.BUILD_URL}"
        }
    }
}
