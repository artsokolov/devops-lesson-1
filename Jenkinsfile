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
                sh "CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build main.go -o main"
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
	        withKubeConfig([credentialsId: 'jenkins-kube-token', serverUrl: 'https://kubernetes:6443']) {
			sh 'kubectl apply -f k8s/pod.yaml'
		}
            }
        }
    }
}
