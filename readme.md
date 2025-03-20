# NSCC Ansible Lab Env
https://redmondo.notion.site/NETW3500-Ansible-Automation-b7bb7bb062454cb0bf4f7d636c32ae4f
## Overview
+---------------------+
| Host Machine        |
| (Windows/Your OS)   |
+---------------------+
|         | Runs      |
|         v           |
+---------------------+
| WSL (Ubuntu)        |
| (Ansible Control)   |
+---------------------+
|         | Manages   |
|         v           |
+---------------------+
| Docker Desktop      |
+---------------------+
|         | Runs      |
|         v           |
+-------------------------------------------------+
| Docker Containers (Target Nodes)               |
| +----------------+ +----------------+ +----------------+ |
| | Ubuntu Container | | Ubuntu Container | | Ubuntu Container | |
| | (Nginx)        | | (Nginx)        | | (Nginx)        | |
| +----------------+ +----------------+ +----------------+ |
+-------------------------------------------------+

### WSL2 Installation
For Windows hosts Only: Follow MS Dcoumentation 
`wsl -l -v`

`wsl --set-version Ubuntu 2`


### Docker Installation
 Follow Docker Documentation or

`winget install Docker.DockerDesktop`
 
 For Windows Hosts: Ensure WSL2 is Integrated


### Ansible Installation (on your Ubuntu WSL2 instance)
`sudo apt update`

`sudo apt install ansible`

### Create SSH Key (optional)
This repo includes a key that will be used with the client (see the dockerfile). Obvioulsy this is not secure, so if you prefer to use you own you can generate it youself and replace the prvoded key in ansible-nginx-demo/ssh:
`ssh-keygen -f ubuntu`

### Provisioning Clients
Copy/Create the provided Dockerfile and run the following command in the same directoy to create your image:
`docker build -t ansible-lab-ubuntu .`

### docker build , build, build
Create a client instance:
`docker run --name ansible-ubuntu-1 -d -p 1022:22 -p 1080:80 ansible-lab-ubuntu`
And another!
`docker run --name ansible-ubuntu-2 -d -p 2022:22 -p 2080:80 ansible-lab-ubuntu`
And another!
`docker run --name ansible-ubuntu-3 -d -p 3022:22 -p 3080:80 ansible-lab-ubuntu`

### Add Clients to SSH Hosts
(if neeed, make the .ssh directory)
`cd ~ && mkdir .ssh`
`sudo nano ~/.ssh/config`

``` 
Host ansible-ubuntu-1
        HostName localhost
        User itstudent
        Port 1022
        IdentityFile ~/ansible_lab/docker/ssh/ubuntu

Host ansible-ubuntu-2
        HostName localhost
        User itstudent
        Port 2022
        IdentityFile ~/ansible_lab/docker/ssh/ubuntu

Host ansible-ubuntu-3
        HostName localhost
        User itstudent
        Port 3022
        IdentityFile ~/ansible_lab/docker/ssh/ubuntu

Host winhost
        Hostname 172.16.147.1
        User    administrator
```

### Add Hosts to Ansible Inventory

See example: inventory.ini


### NGINX Demo
#### Manually? (Don't do this)
We COULD manually complete each of thefollowing configuration changes to set up our web server...

`curl -L https://github.com/do-community/html_demo_site/archive/refs/heads/main.zip -o html_demo.zip`

`sudo apt install unzip`

`unzip html_demo.zip`

`ls -la html_demo_site-main`

`mkdir files`

`nano files/nginx.conf.j2`

```
server {
  listen 80;

  root {{ document_root }}/{{ app_root }};
  index index.html index.htm;

  server_name {{ server_name }};
  
  location / {
   default_type "text/html";
   try_files $uri.html $uri $uri/ =404;
  }
}
```

#### Or using a playbook (Do this!)
Take a look at the provided playbook for this nginx demo. We can use built in modules to complete the installation and copy the configuration! This ensures a consistant, repeatable deployment with ease!

Find the playbook file... if using this repo it should be in ansible_lab/ansible-nginx-demo/

`cd ~/ansible_lab/ansible-nginx-demo`

`cat playbook.yml`

next take a looak at your inventory.ini

`cat inventory.ini`

AND! we can accomodate various states accross multiple servers by including them in our inventory and defining the desired state in our playbooks. Let's run our example playbook:

`ansible-playbook -i inventory.ini playbook.yml`

#### Test your web servers
On your local machine you should be able to hit each server in your browser:
i.e.
http://localhost:1080
http://localhost:2080
http://localhost:3080
