pipeline {
    agent any

    tools {
       go "1.24.1"
    }

    stage('Test') {
        steps {
           sh "go test ./..."
        }
    }

    stages {
        stage('Build') {
            steps {
                sh "go build main.go"
            }
        }
    }
}
