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

        stage('Deploy') {
            steps {
                sshagent(credentials: ['app-server-ssh-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@172.31.13.132 << 'EOF'
                        set -e

                        docker pull otere/node-cls:latest
                        docker stop nodeapp || true
                        docker rm nodeapp || true

                        docker run -d \
                          --name nodeapp \
                          --restart unless-stopped \
                          -p 3000:3000 \
                          otere/node-cls:latest

                        docker ps
EOF
                    '''
                }
            }
        }
    }
}
