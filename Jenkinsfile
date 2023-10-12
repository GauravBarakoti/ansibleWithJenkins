steps {
  sshagent(['controlNode']) {
    sh '''sudo apt update
          #sudo apt-get install ansible
          #sudo apt-get install python3.6
          #apt install python3-pip
          #pip install boto
          git clone https://github.com/GauravBarakoti/ansibleWithJenkins.git
          cd ansibleWithJenkins
          ansible-playbook playbook.yml'''
  }
}