pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Pull the code from your repository
                git url: 'https://github.com/yourusername/your-laravel-project.git', branch: 'main'
            }
        }
        stage('Install Laravel Dependencies') {
            steps {
                script {
                    // Ensure Ansible is installed in the Jenkins environment
                    sh 'apt-get update && apt-get install -y ansible'

                    // Run the Ansible playbook to install dependencies
                    sh 'ansible-playbook -i ansible/inventory ansible/install_laravel_dependencies.yml'
                }
            }
        }
    }
    post {
        always {
            echo 'Cleaning up...'
            // Any cleanup steps if necessary
        }
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
