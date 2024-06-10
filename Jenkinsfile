pipeline {
    agent any // Use any available agent

    triggers {
        pollSCM('H/5 * * * *') // Poll the SCM every 5 minutes
    }

    stages {
        stage('Checkout') {
            agent any // Use any available agent for this stage
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

        stage('Maven Install') {
            agent {
                docker {
                    image 'maven:3.5.0'
                }
            }
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build') {
            agent any // Use any available agent for this stage
            steps {
                echo "Building the project.."
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('Test') {
            agent any // Use any available agent for this stage
            steps {
                echo "Running tests.."
                sh 'mvn test'
            }
        }

        stage('Package') {
            agent any // Use any available agent for this stage
            steps {
                echo 'Packaging the application..'
                sh 'mvn package'
            }
        }

        stage('Deploy') {
            agent any // Use any available agent for this stage
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
}
