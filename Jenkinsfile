pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('post-allure-results') {        
            steps { script {
              allure([
                  results: [
                      [path: './allure-results']
                  ],
                  reportBuildPolicy: 'ALWAYS'
              ])
            }}
        }
        stage('package and sonar-qube-analyze') {
            steps {
                withSonarQubeEnv('MySonar') {
                    sh 'mvn clean package sonar:sonar'
                }
            }
        }
    }
}
