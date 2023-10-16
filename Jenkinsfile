pipeline {
  agent any
  stages {
    stage ( 'Execute Ansible playbook' ) {
      steps {
        sshagent(['controlNode']) {
          // command is used for making an SSH connection with a remote server
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} git clone https://github.com/GauravBarakoti/ansibleWithJenkins.git'
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} "cd ansibleWithJenkins;  sudo mv * /home/ubuntu; "'
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} ansible-playbook -i inventory.yml playbook.yml'
          sh 'sleep 20'
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@${ip} ansible-playbook -i inventory.yml webserver.yml'
        }
      }
    }
  }
}