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
    stage ( 'Execute Ansible playbook' ) {
      steps {
        sshagent(['controlNode']) {
          // command is used for making an SSH connection with a remote server
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} git clone https://github.com/GauravBarakoti/ansibleWithJenkins.git'
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} "cd ansibleWithJenkins;  sudo mv playbook.yml ./.."'
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} ansible-playbook playbook.yml'
        }
      }
    }
  }
}