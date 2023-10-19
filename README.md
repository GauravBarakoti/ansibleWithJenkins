# Ansible Configuration Using Jenkins

You can use this cool feature for visitors count on your profile:
```
![Visitor Count](https : //profile-counter.glitch.me/{YOUR USER}/count.svg)
```

This documentation will help you understand the configuration of 2 EC2 instances on aws and configuring them as apache server (using `dynamic inventory`) with custom webpage that will show the Private and Public IP of the slave node whose ip you are hitting.

And Further also we will explore about creating ec2 with diffenet tags for `test` and `prod` environment.

We'll start from the very basics. And we'll setup dynamic inventory in order to complete our goals.

## Table of Contents
- [Installing Requirements](#installing-requirements)
- [Permissions to access AWS](#step-2-provide-your-aws-credentials)
- [Playbooks Needed](#step-3-create-playbooks-and-ansiblecfg-file)
- [Jenkins Deployment](#step-4-jenkins-deployment)




## Installing Requirements

Step 1: Install all your requirements for dynamic inventory using aws_ec2 ansible plugins:

    You must have the following versions of software packages:

    - python ≥ 3.10
    - boto3 ≥ 1.18.0
    - botocore ≥ 1.21.0
    - Install ansible on your machine with the following commands:


```
 sudo apt update
 ```
```
sudo apt install python3-pip
sudo pip install boto boto3

```

 ```
 sudo apt install software-properties-common
 ```
 ```
 sudo apt-add-repository --yes --update ppa:ansible/ansible
 ```
 ```
 sudo apt install ansible
```

**Note: Your Ansible version must be higher than 2.9.* in order for the AWS EC2 plugin to function.

If you use the Ansible ppa for installation, install pip using the following command:

```
sudo apt-get install python-boto3
```
Install amazon.aws collection using the ansible-galaxy command:

```
ansible-galaxy collection install amazon.aws
```

## Step 2: Provide your AWS Credentials 

On Ansible Contol Node:

Use either `aws configure`  or export the `aws_access_key`, `aws_secret_key` to environment variables

By default, these cred will be saved in default profile but we will change the profile to `devops`.

```
aws configure --profile devops
```
we will use this profile for creating instances on `AWS`.

## Step 3: Create Playbooks and ansible.cfg file

Here, I have two playbooks.

- First is for creating 2 EC2 instances and, 
- Second is to configure them as a webserver by installing apache on both.

Add on, our playbook will have the ec2 configuration and variables that will be passed through the Jenkins Environment Variables.

And, In the second playbook we also want to change the document root for our apache.

First playbook name is `playbook.yml`. And, second playbook is `webserver.yml` {view them for reference.}

And now we'll create the dynamic inventory file named as `inventory.aws_ec2.yml` `aws_ec2.yml is required` in the name of dynamic inventory file without this it will not work.

```
---
plugin: aws_ec2
# aws pulgin for ec2
regions:
  - us-east-1
# by default it will search in all region, but you can specify a specific region as well.
filters:
  instance-state-name : running
keyed_groups:
  - key: tags
    prefix: tag
# a way to group your instances by name or OS or can be anything
hostnames:
  - ip-address
# output details of inventory will be ip-address
```

NOTE: I have made some changes in my inventory according to the requirement. Remember that by `default` the inventory will show the `running instance only`.

Now one more step is required,

Go to `cd /etc/ansible/`

and `sudo vi ansible.cfg`

and provide the following details:

```
[defaults]
inventory = /home/ubuntu/inventory.aws_ec2.yml
# provide the path to your inventory file

[inventory]
enable_plugins = aws_ec2
```

Now, run the command:

```
ansible-inventory --graph
```

```
ubuntu@ip-x-x-x-x:/etc/ansible$ ansible-inventory --graph
@all:
  |--@aws_ec2:
  |  |--ip-x-x-x-x.ec2.internal
  |  |--ip-x-x-x-x.ec2.internal
  |--@ungrouped:
ubuntu@ip-1x-x-x-x:/etc/ansible$
```

This output confirms that your `dynamic inventory` is created based on your requirements.

Congratulations, your inventory is now ready.

Now we have to setup the Jenkins serrver which will run the playbooks on our Ansible control node.

## Step 4: Jenkins Deployment 

Setup jenkins server using this offical documentation:

https://www.jenkins.io/doc/book/installing/linux/

I have used SSH Agent Plugin you can also use the worker node for running playbooks.

Go to manage jenkins -> Pulgins -> install SSH plugins

Then, provide the private key in the global credentials with username: ubuntu and private_key also.

Manage Jenkins -> Configure -> SSH Agent 

hostname : `ipv4_address of ansible node`

and according to your need you can do more modifications.

We will give the required details of variables the environment variables. I have used the environment variables in the pipeline using `-e` which set additional variables as key=value.



- Create the Jenkins Job using pipeline that will the following work -

1. Get the playbooks from github to your ansible control node.

2. Runs the first playbook that will create the 2 EC2 instances  with the provided environment variables through Jenkins ENV.

3. Wait for them to come up.

4. Configure the apache on both managed node or slave nodes with custom web page.

The Jenkinsfile is provided above you can view it for reference.


In this way, we can achieve Ansible Configuration using Jenkins as a CI/CD.

Feel free to adapt this documentation to your specific requirements and Flask application configuration.


# **Thank You**

I hope you find it useful. If you have any doubt in any of the step then feel free to contact me.
If you find any issue in it then let me know.

<!-- [![Build Status](https://img.icons8.com/color/452/linkedin.png)](https://www.linkedin.com/in/gaurav-barakoti-27002223b) -->


<table>
  <tr>
    <th><a href="https://www.linkedin.com/in/gaurav-barakoti-27002223b" target="_blank"><img src="https://img.icons8.com/color/452/linkedin.png" alt="linkedin" width="30"/><a/></th>
    <th><a href="mailto:bestgaurav1234@gmail.com" target="_blank"><img src="https://img.icons8.com/color/344/gmail-new.png" alt="Mail" width="30"/><a/>
</th>
  </tr>
</table>









