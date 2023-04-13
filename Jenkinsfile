pipeline {
    agent any
    stages {
        stage ('Code build and Static Code Analysis'){
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
        stage ('Docker image build and push to docker hub') {
            steps {
                script{
                    sh 'cp -r ../web-app@2/target .'
                    sh 'docker build . -t sudarshantevari/web-app:$BUILD_NUMBER'
                    withCredentials([string(credentialsId: 'docker_password', variable: 'docker_password')]) {
                        sh '''
                        docker login -u sudarshantevari -p $docker_password
                        docker push sudarshantevari/web-app:$BUILD_NUMBER
                        '''
                    } 
                }
            }
        }
        stage ('update deployment file for GitOps') {
            environment {
                GITHUB_USER = "sudarshantevari"
            }

            steps {
                withCredentials([string(credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN')]) {
                    sh '''
                    cat deployment.yaml
                    sed -i "s/docker_tag/$BUILD_NUMBER/" deployment.yaml
                    cat deployment.yaml

                    git clone https://github.com/SudarshanTevari/webapp-config-files.git
                    cp deployment.yaml webapp-config-files/yaml-manifest/
                    cd webapp-config-files/yaml-manifest/

                    git config user.name ${GITHUB_USER}
                    git config user.email ${GITHUB_USER}@gmail.com

                    git add deployment.yaml
                    git commit -m "updated deployment file with ${BUILD_NUMBER}"
                    git push https://${GITHUB_TOKEN}@github.com/${GITHUB_USER}/webapp-config-files HEAD:main
                    cd ../../ && rm -rf webapp-config-files
                    '''
                }
            }
        }
    }
}
