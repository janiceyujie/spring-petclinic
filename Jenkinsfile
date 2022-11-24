pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
//                 git 'https://github.com/janiceyujie/spring-petclinic.git'
                sh './mvnw clean compile'
            }
        }
        stage('Test') {
            steps {
                sh './mvnw test'
            }

            post {
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
            }
        }
        stage('Publish') {
            steps {
                sh './mvnw package'
            }
            post {
                success {
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        stage('Execute ansible playbook') {
            steps {
                ansiblePlaybook becomeUser: 'vagrant', credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: '/etc/ansible/hw2.yaml'
            }
        } 
    }
}
