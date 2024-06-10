pipeline {
   environment {
        JAVA_TOOL_OPTIONS = "-Duser.home=/home/jenkins"
    }
   agent {
        node {
            label 'docker-agent-maven'
        }
    }

    triggers {
        pollSCM('H/5 * * * *') // Poll the SCM every 5 minutes
    }
    stages {
        stage('Checkout') {
            steps {
                // Ensure the repository is cloned into the workspace
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/develop-6.2.x']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/diorsjoy/DemoSite.git',
                        refspec: '+refs/heads/main:refs/remotes/origin/main'
                    ]]
                ])
            }
        }
        stage('Build') {
            steps {
                echo "Building the project.."
                sh '''
                mvn clean install -DskipTests
                '''
            }
        }
        stage('Test') {
            steps {
                echo "Running tests.."
                sh '''
                mvn test
                '''
            }
        }
        stage('Package') {
            steps {
                echo 'Packaging the application..'
                sh '''
                mvn package
                '''
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the application..'
                sh '''
                cp target/DemoSite.war /home/jenkins/
                '''
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
}
