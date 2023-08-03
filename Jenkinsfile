pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("806126151129.dkr.ecr.us-west-1.amazonaws.com/netflix-projects:latest")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 806126151129.dkr.ecr.us-west-1.amazonaws.com/netflix-projects:v1'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://806126151129.dkr.ecr.us-west-1.amazonaws.com/netflix-projects:v1', 'ecr:us-west-1:Tobi-ECR') {
                    // build image
                    def myImage = docker.build("806126151129.dkr.ecr.us-west-1.amazonaws.com/netflix-projects:v1")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
