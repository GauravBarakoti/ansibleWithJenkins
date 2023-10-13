pipeline {
  agent any
  stages {
    // stage ( 'configure dependencies' ) {
    //   steps {
    //     sshagent(['controlNode']) {
    //       sh '''
    //       ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} sudo apt update
    //       ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} sudo apt-get install ansible -y
    //       ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} apt install python3-pip -y
    //       ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} pip install boto
    //       ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} sudo mkdir /etc/ansible/
    //       '''
    //     }
    //   }
    // }
    stage ( 'Storing AWS Cred in ENV' ) {
      steps {
        sshagent(['controlNode']) {
          // command is used for making an SSH connection with a remote server
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} export AWS_ACCESS_KEY_ID=${ACCESS_KEY}'
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} export AWS_SECRET_ACCESS_KEY=${SECRET_KEY}'
          // command is used for making an SSH connection with a remote server
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} git clone https://github.com/GauravBarakoti/ansibleWithJenkins.git'
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} cd ansibleWithJenkins'
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} sudo mv ansible.cfg /etc/ansible/'
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} ansible-playbook playbook.yml'
        }
      }
    }
  }
}