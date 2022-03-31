# NSCC Ansible Lab Env
## Host Configuration

### WSL2 Installation
Follow MS Dcoumentation 

### Docker Installation
 Follow Docker Documentation
 Ensure WSL2 is Integrated

## WSL Ansible Controller:
Tested on Ubuntu 20.04
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
