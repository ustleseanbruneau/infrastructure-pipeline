properties([pipelineTriggers([githubPush()])])
node('linux') {
  git url: 'https://github.com/ustleseanbruneau/infrastructure-pipeline.git', branch: 'master'
  environment {
      NEW_INST_ID = ''
  }
  stage('Test') {
    sh "env"
  }
  stage ("GetInstances") {
    sh "aws ec2 describe-instances --region us-east-1"
  }
  stage ("CreateInstance") {
    def newInst = sh(returnStdout: true, script: "aws ec2 run-instances --image-id ami-467ca739 --count 1 --instance-type t2.micro --key-name awsuspsustkey --security-group-ids sg-065d054f --subnet-id subnet-d7d03f9d --region us-east-1 | jq -r .'Instances[].InstanceId'").trim()
    env.NEW_INST_ID = newInst
  }
  stage ("DeleteInstance") {
    echo "Wait for new EC2 instance to start - instance id: ${env.NEW_INST_ID}" 
    sh "aws ec2 wait --region us-east-1 instance-running --instance-ids ${env.NEW_INST_ID}"
    echo "Terminate new EC2 instance - instance id: ${env.NEW_INST_ID}" 
    sh "aws ec2 terminate-instances --region us-east-1 --instance-ids ${env.NEW_INST_ID}"
  }
}
