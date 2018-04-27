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
    def newInst = sh(returnStdout: true, script: "aws ec2 run-instances --image-id ami-467ca739 --count 1 --instance-type t2.micro --key-name awsuspsustkey --security-group-ids sg-01240748 --subnet-id subnet-c16e9ca6 --region us-east-1 | jq -r .'Instances[].InstanceId'").trim()
    sh "aws ec2 wait --region us-east-1 instance-running --instance-ids '${newInst}'"
  }
  stage ("DeleteInstance") {
    sh "env"
  }
}
