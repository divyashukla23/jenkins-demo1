pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                echo "Checking out source code..."
            }
        }

        stage('Build') {
            steps {
                echo "Building Application..."
            }
        }

        stage('Test') {
            steps {
                echo "Running Tests..."
            }
        }

    }

    post {
        success {
            echo "Build Successful!"
        }
    }
}