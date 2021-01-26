pipeline{
    agent{
        label "master"
    }
    stages{
        stage("Git Checkout"){
            steps{
                echo "Git checkout"
            }
        }
        stage("Build Cloudformation template")
        {
            steps{
                sh '''
                if aws cloudformation describe-stacks  --query 'Stacks[].StackName' | grep Rest-API ; then
                    aws cloudformation update-stack --stack-name Rest-API --template-body file://Rest-API.yml || true
                else
                    aws cloudformation create-stack --stack-name Rest-API --template-body file://Rest-API.yml 
                fi
                '''
            }
        }
        stage("Deploy Rest API"){
            steps{
                    sh '''
                    #!/bin/bash
                    REST_API_ID=$(aws apigateway get-rest-apis --query 'items[?starts_with(name,`TestRestApi`)].id')
                    REST_API_ID=$(echo $REST_API_ID | sed 's/[^a-zA-Z0-9]//g')
                    aws apigateway put-rest-api --cli-binary-format raw-in-base64-out --rest-api-id $REST_API_ID --fail-on-warnings --mode merge --body "file://OpenapiFive.json"
                    '''
            }
        }
        stage("Deploy API gateway to Dev Environment"){
            steps{
                sh '''
                aws apigateway create-deployment --region us-east-1 --rest-api-id $REST_API_ID --stage-name dev5
                '''
            }         
        }
    }
}