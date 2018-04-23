properties([pipelineTriggers([githubPush()])])
node('linux') {
  git url: 'https://github.com/ustleseanbruneau/infrastructure-pipeline.git', branch: 'master'
  stage('Test') {
    sh "env"
  }
  stage ("GetInstances") {
    sh "aws ec2 describe-instances --region us-east-1"
  }
  stage ("CreateInstance") {
    def output = sh returnStdout: true, script: 'aws ec2 run-instances --image-id ami-467ca739 --count 1 --instance-type t2.micro --key-name awsuspsustkey --security-group-ids sg-c01f6789 --subnet-id subnet-29e6174e --region us-east-1 | jq .'
  }
  stage ("DeleteInstance") {
    
    sh "aws ec2 wait --region us-east-1 instance-running --instance-ids $output"
    
    sh "aws ec2 terminate-instances --instance-ids $output"
}
