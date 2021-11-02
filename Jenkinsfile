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
                    git credentialsId: 'sanjayrohilla13', branch: 'master', url: 'https://github.com/sanjayrohilla13/ecr-upgarde.git', poll: false
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
                    aws ecr get-login-password --region ap-southeast-2 | docker login --username AWS --password-stdin 240979667302.dkr.ecr.ap-southeast-2.amazonaws.com
                '''
                }    
            }
        }

        stage('Pull ECR Image') {
            steps {
                echo 'Pushing to ECR....'
                sh '''
                    docker pull 240979667302.dkr.ecr.ap-southeast-2.amazonaws.com/centos-repo:latest
                '''
            }
        }
    }
}
