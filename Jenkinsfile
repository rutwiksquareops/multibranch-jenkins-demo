pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {
        stage('Run Date Command') {
            steps {
                sh 'date'
                sh 'ls -l'
            }
        }
    }
}
