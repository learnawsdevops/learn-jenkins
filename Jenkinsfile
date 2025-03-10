pipeline {
    agent any  // Runs on any available agent

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Git branch to build')
        choice(name: 'DEPLOY_ENV', choices: ['dev', 'staging', 'prod'], description: 'Deployment environment')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run tests before deployment')
    }

    environment {
        PROJECT_NAME = "MyApp"
        BUILD_DIR = "build"
    }

    options {
        timeout(time: 10, unit: 'MINUTES')  // Set timeout for pipeline
        retry(2)  // Retry the pipeline twice if it fails
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: params.BRANCH_NAME, url: 'https://github.com/learnawsdevops/terraform.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'echo "Building the project..."'
                    sh 'mkdir -p $BUILD_DIR'
                    sh 'echo "test"'
                    
                }
            }
        }

        stage('Test') {
            when {
                expression { return params.RUN_TESTS }  // Run tests only if enabled
            }
            steps {
                script {
                    try {
                        sh 'echo "Running tests..."'
                        sh 'exit 0'  // Simulate test success
                    } catch (Exception e) {
                        error("Tests failed!")  // Fail pipeline on test failure
                    }
                }
            }
        }

        stage('Deploy') {
            when {
                branch params.BRANCH_NAME  // Deploy only for the selected branch
            }
            steps {
                sh "echo 'Deploying to ${params.DEPLOY_ENV} environment...'"
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully! ✅"
        }
        failure {
            echo "Pipeline failed! ❌"
        }
        always {
            echo "Cleaning up workspace..."
            
        }
    }
}
