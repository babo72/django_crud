pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "Building..${env.BUILD_ID} on ${env.BUILD_URL}"
                git (
                    url: 'https://github.com/babo72/django_crud.git',
                    branch: 'master',
                    credentialsId: 'babo72-github-token',
                    changelog: true,
                    poll: true
                )
            }
        }
        stage('Test') {
            steps {
                echo "Testing..${env.BUILD_ID} on ${env.BUILD_URL}"
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying....${env.BUILD_ID} on ${env.BUILD_URL}"
            }
        }
    }
}
