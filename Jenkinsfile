pipeline {
  agent any
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        AWS_ACCESS_KEY_ID = credentials('aws-key')
        AWS_SECRET_ACCESS_KEY = credentials('aws-key')
        LAMBDA_BUCKET = 'sahil-bucket-cf-1'
        STACK_NAME = 'cicd-stack'         
        TEMPLATE_FILE = 'template.yaml'  
    }
    stages {
        stage('Python unit test') {
            agent {
                label 'dockeragent'
            }
            steps {
                echo 'Hello, Python Tester'
                git branch: 'main', url: 'https://github.com/Sahilrathee07/jenkins1.git'
                sh 'ls'
                sh 'python3 -m unittest'
            }
        }
        
        stage('deploy') {
            agent {
                label 'dockeragent'
            }
            steps {
                echo 'Hello, Python deployer'
                git branch: 'main', url: 'https://github.com/Sahilrathee07/jenkins1.git'
                sh 'ls'
                script {
                    sh "aws cloudformation package --template-file ${TEMPLATE_FILE} --s3-bucket ${LAMBDA_BUCKET} --output-template-file packaged-template.yaml"
                    sh "aws cloudformation deploy --template-file packaged-template.yaml --stack-name ${STACK_NAME} --capabilities CAPABILITY_IAM"
                }
            }
        }
    }
}
