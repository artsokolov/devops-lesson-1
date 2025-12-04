pipeline {
    agent any

    tools {
       go "1.24.1"
    }

    stages {
        stage('Test') {
            steps {
               sh "go test ."
            }
        }

        stage('Build') {
            steps {
                sh "go build main.go"
            }
        }

        stage('Add known host') {
            steps {
                sh "mkdir -p ~/.ssh"
                sh "ssh-keyscan -H target >> ~/.ssh/known_hosts"
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: '1ccaf00e-e031-4b52-9fc1-1b69a35f65b2', keyFileVariable: 'private_key', usernameVariable: 'username')]) {
                    sh "scp -i ${private_key} main ${username}@target:~"
                }
            }
        }
    }
}
