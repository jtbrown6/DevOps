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

### Basic Webserver Configuration

Default files lie in the `/etc/nginx` folder. Main server configuration happens in `nginx.conf` and virtual hosts go into `conf.d` folders

Let’s take a look at what’s in the default configuration file: /etc/nginx/nginx.conf
<details>


user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;


log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                  '$status $body_bytes_sent "$http_referer" '
                  '"$http_user_agent" "$http_x_forwarded_for"';

access_log  /var/log/nginx/access.log  main;

sendfile        on;
#tcp_nopush     on;

keepalive_timeout  65;

#gzip  on;

include /etc/nginx/conf.d/*.conf;

}

Let’s unpack this section by section to understand what’s going on. The first few lines of the file exist outside of any specific context:
```
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;
```

user directive - specifies which user the worker processes should run under.

worker_processes directive - lets us specify the number of worker processes to use.

error_log directive - sets the file to log errors and what level of messages to log.

pid directive - defines the files that store the PID of the main process.

The next section is our first “context” called events.

events {
    worker_connections  1024;
}

A context in NGINX is expressed using curly braces ({ and }), and certain directives only work in very specific types of contexts. The events context exists to allow us to specify how a worker process should handle connections. In this case, the worker_connections specifies the maximum number of connections that a single worker can have open at one time.

The http Context
The remainder of the default /etc/nginx/nginx.conf file is a single http context. Here’s the entire section:

```
http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                  '$status $body_bytes_sent "$http_referer" '
                  '"$http_user_agent" "$http_x_forwarded_for"';

access_log  /var/log/nginx/access.log  main;

sendfile        on;
#tcp_nopush     on;

keepalive_timeout  65;

#gzip  on;

include /etc/nginx/conf.d/*.conf;

}
```

The first directive that we see is include, which lets us separate our configurations into different files and load them in where we need them. The mime.types file defines the content return types that should be used for various files that the server might render. The default_type directive states the content type to use if the content is not defined in the types file. The next two directives are all about logging. Looking at the log_format directive, we see a lot of tokens that start with a dollar sign ($). These tokens are variables that are available when a request is being processed, and this directive defines which should be used (and in what position) to log the request information. access_log defines the file to log request information to.

The next directive, sendfile is a little complicated, but it specifies whether or not to use the sendfile() system call when delivering content. keepalive_timeout sets how long to hold onto the TCP connection for a client before opening up the connection for a new client. Lastly, the include directive is used one last time to load all of the .conf files in the /etc/nginx/conf.d/ directory. This is where we’ll be defining our virtual hosts as we learn throughout this course.
</details>

