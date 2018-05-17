pipeline {
    agent any

    stages {
        stage('Cloning Repo') {
            steps {
                echo 'Cloning Repo...'
                git 'https://github.com/ismc/an-demo-kit'
            }
        }
        stage('Build Cloud') {
            steps {
                echo 'Building Cloud...'
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-creds']]) {
                  ansiblePlaybook colorized: true, disableHostKeyChecking: true, extras: 'cloud_model=an-demo1,cloud_project=scarter-jenkins', playbook: 'build-demo.yml'
                  sh 'env'
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Destroy Cloud') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
