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
                    sh 'mvn clean package spring-boot:repackage sonar:sonar'
                }
            }
        }
        stage('deploy-with-ansible') {
            environment {
                ANSIBLE_INVENTORY = credentials('ansible-inventory-v1')
                ANSIBLE_VAULT_PASSWORD_FILE = credentials('ansible-vault-password-file-v1')
            }
            steps {
                sh 'ansible-playbook --vault-password-file=$ANSIBLE_VAULT_PASSWORD_FILE -i $ANSIBLE_INVENTORY deploy/play-install.yml'
            }
        }
    }
}
