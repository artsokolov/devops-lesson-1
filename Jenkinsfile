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
                sh "scp -i ${private_key} main ${username}@target:~"
            }
        }
    }
}
