pipeline {
    agent {
        node {
            label 'docker-agent-maven'
        }
    }
    triggers {
        pollSCM('H/5 * * * *') // Poll the SCM every 5 minutes
    }
    environment {
        MAVEN_OPTS = "-Xmx2g"
    }
    stages {
        stage('Checkout') {
            steps {
                // Ensure the repository is cloned into the workspace
                checkout([
                    $class: 'GitSCM',
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/BroadleafCommerce/DemoSite.git',
                        refspec: '+refs/heads/*:refs/remotes/origin/*'
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
                cp target/DemoSite.war 
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
