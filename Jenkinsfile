pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh 'mvn compile'
                sh 'mvn test'
                sh 'mvn package'
            }
        }
    }
}
