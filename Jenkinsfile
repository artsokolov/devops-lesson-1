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

	stage('Build docker image') {
	    steps {
	    	sh "docker build . --tag ttl.sh/superpupergoapp:2h"
		sh "docker push ttl.sh/superpupergoapp:2h"
	    }
	}

        stage('Deploy') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: '900bc006-9472-4825-afa9-ed5831dba305', keyFileVariable: '', usernameVariable: 'username')]) {
		    sh 'ssh -i ${private_key} ${username}@docker "docker run --pull always -p 4444:4444 -d ttl.sh/superpupergoapp:2h"'
                }
            }
        }
    }
}
