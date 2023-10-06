pipeline {
    agent any

    stages {
        stage('checkout') {
            steps {
                echo "Building..${env.BUILD_ID} on ${env.BUILD_URL}, ${env.WORKSPACE}"
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
        stage('analysis') {
            steps {
                script {
                    scannerHome = tool 'Sonar-Scanner'
                }
                withSonarQubeEnv('SonarCloud-babo72'){
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.organization=babo72 -Dsonar.projectKey=babo72_github-django-example -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=0b487c792ca10ab464af4854b119373c88f894cb"
                }
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying....${env.BUILD_ID} on ${env.BUILD_URL}"
            }
        }
    }
}
