pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('vcs') {
            steps {
                git branch: 'dev', url: 'https://github.com/rkreddy8096/saleor-dashboard.git'
            }
        }
        stage('docker image build') {
            steps {
                sh 'docker image build -t reddy8096/saleor-dashboar:DEV .'
            }
        }
        stage('push image to registry') {
            steps {
                sh 'docker image push reddy8096/saleor-dashboar:DEV'
            }
        }
        
        stage('create terraform infrastructre') {
            agent { label 'build' }
            steps {
                sh 'git clone https://github.com/hashicorp/learn-terraform-provision-eks-cluster'
                sh 'cd /learn-terraform-provision-eks-cluster'
                sh 'terraform init && terraform apply -auto-approve'
            }
        }
    }
}
