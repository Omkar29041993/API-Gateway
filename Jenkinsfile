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
                    aws cloudformation update-stack --stack-name Rest-API --template-body file://Rest-API.yml
                else
                    aws cloudformation create-stack --stack-name Rest-API --template-body file://Rest-API.yml
                fi
                '''
            }
        }
    }
}