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
                    REST_API_ID=$(aws apigateway get-rest-apis --query 'items[?starts_with(name,`MyRestAPI2`)].id')
                    aws apigateway put-rest-api --cli-binary-format raw-in-base64-out --rest-api-id $REST_API_ID --fail-on-warnings --mode merge --body "file://OpenapiThree.json"
                    '''
            }
        }
    }
}