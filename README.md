# Ansible Configuration Using Jenkins


Visits:

![Visitor Count](https://profile-counter.glitch.me/gauravbarakoti/count.svg)

```
![Visitor Count](https : //profile-counter.glitch.me/{YOUR USER}/count.svg)
```

This documentation will help you understand the configuration of 2 EC2 instances on aws and configuring them as apache server with custom webpage that will show the Private and Public IP of the slave node whose ip you are hitting.

We'll start from the very basics. And we'll setup dynamic inventory in order to complete our goals.

## Step 1: Install all your requirements for dynamic inventory using aws_ec2 ansible plugins:

    You must have the following versions of software packages:

    - python ≥ 3.10
    - boto3 ≥ 1.18.0
    - botocore ≥ 1.21.0
    - Install ansible on your machine with the following commands:


```
 sudo apt update
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

## Step 3: Create Playbooks and ansible.cfg file

Here, I have two playbooks.

- First is for creating 2 EC2 instances and, 
- Second is to configure them as a webserver by installing apache on both.

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














