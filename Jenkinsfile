pipeline {
    agent any

   

    stages {
        

        stage('Code Checkout') {
            steps {
                script {
                    checkout(
                        scm: [
                            $class: 'GitSCM',
                            branches: [[name: '*/main']],
                            userRemoteConfigs: [[
                                credentialsId: 'github-access-token-1',
                                url: 'https://github.com/JozzRayan/tf-sample.git'
                            ]]
                        ]
                    )
                }
            }
        }

        stage('Pull and Run Terraform Docker Image') {
            steps {
              docker.image('hashicorp/terraform:latest').withRun('-v usr/local/bin:usr/local/bin ''-u 1001:1001')
               { c ->
                sh 'terraform --version'
                }            
            
            }
        }


        stage('Terraform Infra') {
            steps {
                script {
                    def action = params.ACTION

                    // Navigate to the Dev directory and run Terraform commands with AWS credentials
                    dir('Environment/Dev') {
                        sh 'terraform init'
                        sh 'terraform plan'

                        if (action == 'deploy') {
                            sh 'terraform apply -auto-approve'
                        } else if (action == 'destroy') {
                            sh 'terraform destroy -auto-approve'
                        }
                    }
                }
            }
        }
    }
}
