pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Pull the code from your repository
                git url: 'https://github.com/Andualem-Andy/laravel-backend-ci-cd.git', branch: 'main'
            }
        }
        stage('Setup') {
            steps {
                // Ensure composer is installed and then install dependencies
                sh 'composer install'
            }
        }
        stage('Verify Sail') {
            steps {
                // Check if the sail script exists
                sh 'ls -la ./vendor/bin' // List contents of vendor/bin
                sh 'ls -la ./vendor/bin/sail' // Check if sail exists
            }
        }
        stage('Build') {
            steps {
                // Start the Sail environment if necessary
                sh './vendor/bin/sail up -d'
            }
        }
        stage('Install Dependencies') {
            steps {
                // Install Composer dependencies
                sh './vendor/bin/sail composer install'
            }
        }
        stage('Run Tests') {
            steps {
                // Run your PHPUnit tests
                sh './vendor/bin/sail test'
            }
        }
        stage('Run Migrations') {
            steps {
                // Run database migrations
                sh './vendor/bin/sail artisan migrate'
            }
        }
        stage('Start Application') {
            steps {
                // Ensure your application is running
                sh './vendor/bin/sail up -d'
            }
        }
    }
    post {
        always {
            // Clean up Docker containers after build
            sh './vendor/bin/sail down' // Clean up Docker environment with full path
        }
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Build or tests failed!'
        }
    }
}
