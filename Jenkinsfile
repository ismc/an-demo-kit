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
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'Ansible (scarter)']]) {
                  ansiblePlaybook colorized: true, disableHostKeyChecking: true, extras: '-e cloud_model=an-demo1 -e cloud_project=scarter-jenkins', playbook: 'build-demo.yml'
                  sh 'env'
                }
            }
        }
        stage('Run Playbooks') {
            steps {
                echo 'Wait for the Routers to come up...'
                ansiblePlaybook colorized: true, disableHostKeyChecking: true, inventory: 'inventory/scarter-jenkins', playbook: 'common/check-ssh.yml'
            }
            steps {
                echo 'Running network-system.yml...'
                ansiblePlaybook colorized: true, disableHostKeyChecking: true, inventory: 'inventory/scarter-jenkins', playbook: 'net/network-system.yml'
            }
            steps {
                echo 'Running network-security.yml...'
                ansiblePlaybook colorized: true, disableHostKeyChecking: true, inventory: 'inventory/scarter-jenkins', playbook: 'net/network-security.yml'
            }
            steps {
                echo 'Running network-interfaces.yml...'
                ansiblePlaybook colorized: true, disableHostKeyChecking: true, inventory: 'inventory/scarter-jenkins', playbook: 'net/network-interfaces.yml'
            }
            steps {
                echo 'Running network-ipsec-vpn.yml...'
                ansiblePlaybook colorized: true, disableHostKeyChecking: true, inventory: 'inventory/scarter-jenkins', playbook: 'net/network-ipsec-vpn.yml'
            }
            steps {
                echo 'Running network-bgp.yml...'
                ansiblePlaybook colorized: true, disableHostKeyChecking: true, inventory: 'inventory/scarter-jenkins', playbook: 'net/network-bgp.yml'
            }
        }
        stage('Destroy Cloud') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
