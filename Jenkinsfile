pipeline {
  agent any
  stages {
    // stage ( 'configure dependencies' ) {
    //   steps {
    //     sshagent(['controlNode']) {
    //       sh '''
    //       sudo apt update
    //       sudo apt-get install ansible
    //       sudo apt-get install python3.6
    //       apt install python3-pip
    //       pip install boto
    //       '''
    //     }
    //   }
    // }
    stage ( 'cloning the repo which has playbook.yml' ) {
      steps {
        sshagent(['controlNode']) {
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@54.221.46.163 git clone https://github.com/GauravBarakoti/ansibleWithJenkins.git'
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@54.221.46.163 cd ansibleWithJenkins'
          sh 'ssh -tt -o StrictHostKeyChecking=no ubuntu@54.221.46.163 ansible-playbook playbook.yml'
        }
      }
    }
  }
}