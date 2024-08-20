pipeline {
    agent any

    environment {
        // Load AWS credentials from Jenkins credentials store
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        REGION = 'us-west-2'  // Change to your desired AWS region
        BUCKET_NAME = 'my-unique-s3-bucket-name'  // Change to your desired bucket name
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the Git repository
                git branch: 'main', url: 'https://github.com/unas123/testing-terraform.git'
            }
        }

        stage('Terraform Init') {
            steps {
                script {
                    dir('terraform/s3') {
                        sh 'terraform init'
                    }
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    dir('terraform/s3') {
                        sh """
                        terraform plan \
                            -var region=${REGION} \
                            -var bucket_name=${BUCKET_NAME}
                        """
                    }
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    dir('terraform/s3') {
                        sh """
                        terraform apply -auto-approve \
                            -var region=${REGION} \
                            -var bucket_name=${BUCKET_NAME}
                        """
                    }
                }
            }
        }
    }
}
