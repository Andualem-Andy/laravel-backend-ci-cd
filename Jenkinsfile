pipeline {
    agent any // This can be changed to a specific docker agent if needed
    stages {
        stage('Checkout') {
            steps {
                // Pull the code from your repository
                git url: 'https://github.com/Andualem-Andy/laravel-backend-ci-cd.git', branch: 'main'
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
                sh 'sail up -d'
            }
        }
    }
    post {
        always {
            // Clean up Docker containers after build
            sh 'sail down' // Clean up local
        }
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Build or tests failed!'
        }
    }
}
