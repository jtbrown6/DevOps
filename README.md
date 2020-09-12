# Welcome to the DevOps Handbook

## Table of Contents
[Linux Quick Wins](#Linux)

Configuring the iTerm2 Terminal

Setting up Python

[Ansible](/Ansible.md)

[NGINX](/NGINX.md)

## Linux

*Moving local file to ec2-instance*
```
scp file.text ansible:/home/ec2-user
```
*Copy key to remote server*
```
cat ~/.ssh/id_rsa.pub | ssh -i aws.pem ubuntu@<ip> "cat ->> ~/.ssh/authorized_keys2
```
*Checking Versions*
```
$ cat /etc/os-release
$ cat /proc/version
```