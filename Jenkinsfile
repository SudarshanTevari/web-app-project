pipeline {
    agent any
    stages {
        stage ('Quality Gate Status Check'){
            agent {
                docker {
                    image 'maven'
                    args '-v $HOME/.m2:/root/.m2'
                }
            }
            steps {
                script {
                    withSonarQubeEnv('Sonar-server') {
                        sh "mvn sonar:sonar"
                    }
                    timeout(time: 1, unit: 'HOURS') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                    sh "mvn clean install"
                }
            }
        }
        stage ('Docker build') {
            steps {
                script{
                    sh 'cp -r ../web-app@2/target .'
                    sh 'docker build . -t sudarshantevari/web-app:$BUILD_NUMBER'
                    withCredentials([string(credentialsId: 'docker_password', variable: 'docker_password')]) {
                        sh 'docker login -u sudarshantevari -p $docker_password'
                        sh 'docker push sudarshantevari/web-app:$BUILD_NUMBER'
                    } 
                }
            }
        }
    }
}
