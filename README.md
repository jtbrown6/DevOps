# Welcome to the DevOps Handbook

## Table of Contents
[Linux Quick Wins](#Linux)

Configuring the iTerm2 Terminal

Setting up Python

[Setting up Ansible](#Ansible)

## Linux

*Moving local file to ec2-instance*
```
scp file.text ansible:/home/ec2-user
```

## Ansible

Configuring Ansible Objectives:

1. Install Ansible
2. Create the `Ansible User` and give `sudu` rights

Step 1: Installing Ansible

```bash
$ sudo yum install epel-release | sudo amazon-linux-extras install epel
$ sudo yum install ansible | sudo amazon-linux-extras ansible2
$ sudo yum install -y python-boto python-boto3
$ sudo yum install -y python-pip python3-pip
$ sudo pip install awscli
$ sudo pip install boto boto3  -> fix error “boto required forr this module”
```

Step 2: Configure SSH Agent in AWS

*Issue: cannot ping as the current local user to ec2 instance because it needs the key. Links the .pem and use the correct -u for ansible command*

```bash
$ ssh-agent bash
$ chmod 600 ec2-keypair.pem
```
*Note: in the `/etc/ansible/ansible.cfg` file, disable `hostkeychecking` to prevent issues*

Step 3: Create Ansible User

```bash
$ sudo useradd ansible
$ sudo passwd ansible
```
*Note: modify `/etc/ssh/sshdconfig` for password authentication and restart server*

```bash
$ restart systemctl
$ restart sshd.service
```

Step 4: Testing Ansible
```
ssh localhost
ansible all -m ping -u ec2-user
```


