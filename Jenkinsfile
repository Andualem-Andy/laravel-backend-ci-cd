pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Pull the code from the Git repository
                git url: 'https://github.com/Kennibravo/jenkins-laravel.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install Laravel dependencies using Composer 
                sh 'composer install'
            }
        }

        stage('Setup Environment') {
            steps {
                // Copy the example environment file and generate the application key
                sh 'cp .env.example .env'
                sh 'php artisan key:generate'
            }
        }

        stage('Run Tests') {
            steps {
                // Run unit tests with PHPUnit
                sh './vendor/bin/phpunit'
            }
        }
    }

    post {
        success {
            echo 'Build and tests ran successfully!'
        }

        failure {
            echo 'Build or tests failed. Please check the logs for details.'
        }

        always {
            echo 'Cleaning up...'
            // You can add any additional cleanup steps here if needed
        }
    }
}
