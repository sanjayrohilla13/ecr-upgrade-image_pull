pipeline {
    agent any
    environment {
        AWS_DEFAULT_REGION = 'ap-southeast-2'
        //TEMP_VAR = credentials('srv-ecr-usr')
    }

    stages {
        stage('Download GIT Hub Repo') {
            steps {
                echo 'Downloading..'
                script {
                    git credentialsId: 'sanjayrohilla13', branch: 'ecr-login-with-authcode', url: 'https://github.com/sanjayrohilla13/ecr-upgarde.git', poll: false
               }
            }
        }

        // Login in AWS ECR for getting Authentication Token
        stage('Docker Login') {
            steps {
                withCredentials([aws(accessKeyVariable:'AWS_ACCESS_KEY_ID',credentialsId:'srv-ecr-usr',secretKeyVariable:'AWS_SECRET_ACCESS_KEY')]) {
                // some block
                //sh 'make docker-login'
                echo 'Logging in..'
                sh '''
                    aws --version
                    aws_account_id="240979667302"
                    aws_region="ap-southeast-2"
                    ecr_url="${aws_account_id}.dkr.ecr.${aws_region}.amazonaws.com"
                    password="$(aws ecr get-authorization-token --region "${aws_region}" --output text --query 'authorizationData[].authorizationToken' | base64 -d | cut -d: -f2 )"
                    echo "$password" | docker login --password-stdin --username AWS "$ecr_url" 
                '''
                }    
            }
        }

        stage('Pull ECR Image') {
            steps {
                echo 'Pushing to ECR....'
                sh '''
                    docker pull 240979667302.dkr.ecr.ap-southeast-2.amazonaws.com/centos:latest
                '''
            }
        }
    }
}
