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
                aws cloudformation create-stack --stack-name Rest-API --template-body file://Rest-API.yml
                '''
            }
        }
    }
}