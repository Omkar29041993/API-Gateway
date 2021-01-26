pipeline{
    agent{
        label "node"
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
                aws cloudformation update-stack --stack-name Rest-API --template-body file://Rest-API.yml
                '''
            }
        }
    }
}