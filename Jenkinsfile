pipeline {
    agent {
        docker {
            image 'docker:latest' // Use the Docker image
            args '--privileged' // Required for Docker-in-Docker
        }
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull the code from your repository
                git url: 'https://github.com/Andualem-Andy/laravel-backend-ci-cd.git', branch: 'main'
            }
        }
        stage('Setup') {
            steps {
                // Install dependencies using Composer
                sh 'composer install'
            }
        }
        stage('Build') {
            steps {
                // Start the Sail environment
                sh './vendor/bin/sail up -d'
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
    }
    post {
        always {
            // Clean up Docker containers after build
            sh './vendor/bin/sail down' // Clean up Docker environment
        }
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Build or tests failed!'
        }
    }
}
