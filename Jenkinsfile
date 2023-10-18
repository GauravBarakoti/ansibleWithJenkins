pipeline {
  agent any
  stages {
    stage ( 'Execute Ansible playbook' ) {
      steps {
        sshagent(['controlNode']) {
          // command is used for making an SSH connection with a remote server
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} git clone https://github.com/GauravBarakoti/ansibleWithJenkins.git'
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} "cd ansibleWithJenkins; ansible-playbook -i inventory.aws_ec2.yml playbook.yml -e region=$REGION -e instance_type=$INSTANCE -e image_id=$AMI -e key_name=$KEY -e subnet_id=$SUBNET;"'
          sh 'sleep 30'
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} "cd ansibleWithJenkins;ansible-playbook -i inventory.aws_ec2.yml webserver.yml;"'
        }
      }
    }
  }
}