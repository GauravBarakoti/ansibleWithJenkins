pipeline {
  agent any
  stages {
    // stage ( 'configure dependencies' ) {
    //   steps {
    //     sshagent(['controlNode']) {
    //       sh '''
    //       ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} sudo apt update
    //       ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} sudo apt install software-properties-common
    //       ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} sudo apt-add-repository --yes --update ppa:ansible/ansible
    //       ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} sudo apt install ansible
    //       ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} sudo apt-get install python-boto3
    //       '''
    //     }
    //   }
    // }
    stage ( 'Execute Ansible playbook' ) {
      steps {
        sshagent(['controlNode']) {
          // command is used for making an SSH connection with a remote server
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} git clone https://github.com/GauravBarakoti/ansibleWithJenkins.git'
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} "cd ansibleWithJenkins;  sudo mv * /home/ubuntu; "'
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} ansible-playbook -i inventory.yml playbook.yml'
        }
      }
    }
  }
}