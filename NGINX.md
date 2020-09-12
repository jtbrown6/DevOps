# NGINX Web Server Deep Dive
Length: 05:36:00

## Table of Contents
- Introduction to NGINX
- Installing and Running
- Basic Web Server Configuration
- Basic Security
- NGINX Rewrites
- NGINX Modules
- Reverse Proxy
- Load Balancing
- Logging
- Security
- Performance

### Introduction to NGINX

Built in order to solve the problem of C10K (10k Concurrent Sessions), 50% of the top 1000 sites use NGINX. Can be a reverse proxy and load balancer. 

HTTP is a `connectionless` text based protocol where a Web-Browser (Client) sends a request to a Server and once provided, the connection between the two is disconnected and `new connections are made for each new request`.  `Headers` are important because additional information can be passed in the HTTP request or response. 

NGINX vs Apache: NGINX is faster at serving static files but does not have a .htaccess file which allows for directory based access controls vs global access controls.

### Installing and Running (AWS Linux 2 - RHEL Based)

*Note: When standing up new ec2 server, chmod 400 the .pem key*

- Confirm version of AWS AMI using the below command
    ```
    cat /etc/os-release
    ```
- Run AWS based commands and verify version installed correctly
    ```
    sudo amazon-linux-extras install nginx1
    sudo nginx -v 
    ```
- Use systemd to run NGINX as a server to boot when server starts
    ```
    $ sudo systemctl start nginx
    $ sudo systemctl enable nginx
    ```
- Restart server and `navigate to AWS Public IP` to see if NGINX is loaded at front page. Make sure you modify the security group to allow inbound HTTP traffic from 0.0.0.0/0.
