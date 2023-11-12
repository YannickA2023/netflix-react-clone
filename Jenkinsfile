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
                    docker.build("719283100085.dkr.ecr.us-east-2.amazonaws.com/netflix-oct:v2")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 719283100085.dkr.ecr.us-east-2.amazonaws.com/netflix-oct:v2'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://719283100085.dkr.ecr.us-east-2.amazonaws.com/netflix-oct', 'ecr:us-east-2:yannick-ecr') {
                    // build image
                    def myImage = docker.build("719283100085.dkr.ecr.us-east-2.amazonaws.com/netflix-oct:v2")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
