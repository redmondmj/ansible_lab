# NSCC Ansible Lab Env
https://redmondo.notion.site/NETW3500-Ansible-Automation-b7bb7bb062454cb0bf4f7d636c32ae4f
## Linux Host Configuration

### WSL2 Installation
For Windows hosts Only: Follow MS Dcoumentation 

### Docker Installation
 Follow Docker Documentation
 For Windows Hosts: Ensure WSL2 is Integrated

## Ansible Controller:
`sudo apt install ansible`
### Ansible Installation
`sudo apt update`
`sudo apt install ansible`

### Create SSH Key
`ssh-keygen -f ubuntu`

### Provisioning Clients
Copy/Create the provided Dockerfile and run the following command in the same directoy to create your image:
`docker build -t ansible-lab-ubuntu .`

### docker build , build, build
Create a client instance:
`docker run --name ansible-ubuntu-1 -d -p 2022:22 -p 8082:80 ansible-lab-ubuntu`
And another!
`docker run --name ansible-ubuntu-1 -d -p 2023:22 -p 8083:80 ansible-lab-ubuntu`

### Add Clients to SSH Hosts
`sudo nano ~/.ssh/config`

``` 
Host ansible-ubuntu-1
        HostName localhost
        User itstudent
        Port 2022
        IdentityFile ~/ansible_lab/docker/ssh/ubuntu

Host ansible-ubuntu-2
        HostName localhost
        User itstudent
        Port 2023
        IdentityFile ~/ansible_lab/docker/ssh/ubuntu

Host winhost
        Hostname 172.16.147.1
        User    administrator
```

### Add Hosts to Ansible Inventory

See example: inventory.ini


### NGINX Demo
#### Manually?
We COULD manually complete each of the following configuration changes to set up our web server...
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

`nano playbook.yml`

`ansible-playbook -i inventory playbook.yml -u itstudent -K`
