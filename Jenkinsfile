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
