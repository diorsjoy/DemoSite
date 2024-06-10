pipeline {
    agent any

    tools {
        maven "maven"
    }

    stages {
        stage('Build') {
            steps {
                echo "Building the project.."
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('Test') {
            steps {
                echo "Running tests.."
                sh 'mvn test'
            }
        }
        
        stage('Package') {
            steps {
                echo 'Packaging the application..'
                sh 'mvn package'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application..'
                sh 'cp target/DemoSite.war /home/jenkins/'
            }
        }
    }

    post {
        always {
            // Clean up workspace
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        
}
