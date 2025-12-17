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
                sh "ssh-keyscan -H 16.176.23.7 >> ~/.ssh/known_hosts"
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
                withCredentials([sshUserPrivateKey(credentialsId: '77fca62e-71a3-44d2-9874-b3cc1483d832', keyFileVariable: 'private_key', usernameVariable: 'username')]) {
		    sh 'ssh -i ${private_key} ${username}@16.176.23.7 "\
		    	docker rm -f superpupergoapp || true && \
		    	docker run --name superpupergoapp --pull always -p 4444:4444 -d ttl.sh/superpupergoapp:2h \
		    "'
                }
            }
        }
    }
}
