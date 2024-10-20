pipeline {
    agent any // Use a specific agent if possible, e.g., docker { image 'your-php-image' }

    environment {
        COMPOSER_HOME = "${WORKSPACE}/.composer" // Specify Composer home if needed
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
                script {
                    // Check if Composer is installed, and install dependencies
                    if (!fileExists('composer.phar')) {
                        echo 'Composer is not installed. Installing Composer...'
                        sh 'curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer'
                    }
                    sh 'composer install'
                }
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
                // Start the Sail environment
                script {
                    // Start Sail if not already running
                    sh 'if ! ./vendor/bin/sail ps | grep -q "Up"; then ./vendor/bin/sail up -d; fi'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                // Install Composer dependencies using Sail
                sh './vendor/bin/sail composer install'
            }
        }
        stage('Run Tests') {
            steps {
                // Run your PHPUnit tests using Sail
                sh './vendor/bin/sail test'
            }
        }
        stage('Run Migrations') {
            steps {
                // Run database migrations using Sail
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
            script {
                sh './vendor/bin/sail down' // Clean up Docker environment with full path
            }
        }
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Build or tests failed!'
        }
    }
}
