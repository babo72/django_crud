pipeline {
    agent any

    stages {
        stage('checkout') {
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
        stage('analysis') {
            steps{
                script {
                    scannerHome = tool 'Sonar-Scanner'
                }
                withSonarQubeEnv('SonarCloud-babo72'){
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.organization=babo72 -Dsonar.projectKey=babo72_github-django-example"
                    //sh "mvn clean package"
                    //sh "mvn sonar:sonar -Dsonar.projectKey=demo -Dsonar.host.url=http://192.168.123.141:9000 -Dsonar.login=03a3d935387d5a8bb8894ff0a0f282055f39466a"
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
