pipeline {
    stages {
        stage('Preparation') {
            steps {
                sh 'docker rm -f samplerunning || true'
                sh 'rm -rf tempdir'
            }
        }
        stage('Build') {
            steps {
                sh 'bash sample-app.sh'
            }
        }
        stage('Results') {
            steps {
                sshagent(['prod-ssh']) {
                    sh '''
                        mkdir -p ~/.ssh
                        ssh-keyscan -H 192.0.2.10 >> ~/.ssh/known_hosts
                        export DOCKER_HOST=ssh://deploy@192.0.2.10
                        docker rm -f samplerunning || true
                        rm -rf tempdir
                        bash sample-app.sh
                    '''
                }
            }
        }
    }
}