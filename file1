pipeline {
    agent any

    // Define parameters
    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Git branch to build')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'qa', 'prod'], description: 'Environment to deploy to')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run unit tests?')
    }

    environment {
        BUILD_DIR = 'build'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the specified branch
                git branch: "${params.BRANCH_NAME}", url: 'https://github.com/your-repo.git'
            }
        }

        stage('Build') {
            steps {
                // Run the build process
                echo "Building project from branch: ${params.BRANCH_NAME}"
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Test') {
            when {
                expression { return params.RUN_TESTS }
            }
            steps {
                // Run unit tests if RUN_TESTS is true
                echo "Running tests..."
                sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                // Deploy to the selected environment
                echo "Deploying to ${params.ENVIRONMENT} environment..."
                sh "deploy.sh ${params.ENVIRONMENT}"
            }
        }
    }

    post {
        always {
            // Clean up
            echo "Cleaning up..."
            deleteDir()
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
