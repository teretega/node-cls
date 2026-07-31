pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t otere/node-cls:latest .'
            }
        }

        stage('Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                        -u "$DOCKER_USERNAME" \
                        --password-stdin

                        docker push otere/node-cls:latest
                    '''
                }
            }
        }

        stage('Run') {
            steps {
                sh 'docker stop nodeapp || true'
                sh 'docker rm nodeapp || true'
                sh 'docker run -d --name nodeapp -p 3001:3000 otere/node-cls:latest'
            }
        }
    }
}
