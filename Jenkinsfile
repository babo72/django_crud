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
        stage('install Python package') {
            steps {
                echo 'install required packages...'
                sh 'pip3 install -r requirements.txt'
                
                dir('apps') {
                    echo 'run test code...'
                    sh 'python3 manage.py test'
                    
                    echo 'run coverage & report...'
                    // coverage 설치는 requirements.txt 로 옮겨
                    sh 'python3 -m pip install coverage'
                    sh "python3 -m coverage run --source='.' manage.py test"
                    // view coverage result
                    sh 'python3 -m coverage report'
                }
            }
        }
        stage('Test') {
            steps {
                echo "Testing..${env.BUILD_ID} on ${env.BUILD_URL}"
            }
        }
        stage('SonarQube analysis') {
            steps {
                script {
                    scannerHome = tool 'Sonar-Scanner'
                }
                withSonarQubeEnv('SonarCloud-babo72'){
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.organization=babo72 -Dsonar.projectKey=babo72_github-django-example -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=0b487c792ca10ab464af4854b119373c88f894cb"
                }
            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
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
