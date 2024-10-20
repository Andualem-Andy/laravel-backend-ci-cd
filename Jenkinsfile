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
                // Add vendor/bin to the PATH
                sh 'export PATH=$PATH:./vendor/bin'
            }
        }
        stage('Build') {
            steps {
                // Start the Sail environment if necessary
                sh 'sail up -d'
            }
        }
        stage('Install Dependencies') {
            steps {
                // Install Composer dependencies
                sh 'sail composer install'
            }
        }
        stage('Run Tests') {
            steps {
                // Run your PHPUnit tests
                sh 'sail test'
            }
        }
        stage('Run Migrations') {
            steps {
                // Run database migrations
                sh 'sail artisan migrate'
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
            sh './vendor/bin/sail down' // Clean up local
        }
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Build or tests failed!'
        }
    }
}
