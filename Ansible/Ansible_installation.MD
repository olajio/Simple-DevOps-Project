# Ansible Installation

Ansible is an open-source automation platform. It is very, very simple to set up and yet powerful. Ansible can help you with configuration management, application deployment, task automation.

### Pre-requisites

1. An AWS EC2 instance (on Control node)

### Installation steps:
#### on Amazon EC2 instance

1. Install python and python-pip
   ```sh
   yum install python -y
   yum install python-pip -y
   ```
1. Install ansible using pip check for version
    ```sh
    pip install ansible
   ansible --version
   ```
   
1. Create a user called ansadmin (on Control node and Managed host)  
   ```sh
   useradd ansadmin
   passwd ansadmin
   ```
   
1. Below command grant sudo access to ansadmin user. But we strongly recommended using "visudo" command if you are aware vi or nano editor.  (on Control node and Managed host)
   ```sh
   echo "ansadmin ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
   ```
   
1. Configure Ansible Host to allow password authentication for ssh
   ```sh
   sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
   systemctl restart sshd
   ```
1. Log in as a ansadmin user on master and generate ssh key (on Control node)
   ```sh 
   sudo su - ansadmin
   ssh-keygen
   ```
1. From ansible control host, copy keys onto all ansible managed hosts (on Control node)
   ```sh 
   ssh-copy-id ansadmin@<target-server>
   ```
   In the specific case, we copy the keys onto the Ansible host

1. Ansible server used to create images and store on docker registry. Hence install docker, start docker services and add ansadmin to the docker group. 
   ```sh
   yum install docker -y
   
   # start docker services 
   systemctl start docker
   systemctl enable docker
   
   # add user to docker group 
   usermod -aG docker ansadmin

   ```
1. Create a directory /etc/ansible and create an inventory file called "hosts" add control node and managed hosts IP addresses to it. Or if the hostnames are reachable, and /etc/hosts have is updated on the Ansible host, you can just update /etc/ansible/hosts with the hostnames of the target machine
   ```sh
   mkdir /etc/ansible
   touch /etc/ansible/hosts
   echo "docker_host" >> /etc/ansible/hosts
   echo "localhost" >> /etc/ansible/hosts 
   ```
   
   
### Validation test

   
1. Run ansible command as ansadmin user it should be successful (Master)
   ```sh 
   ansible all -m ping
   ```


  ### Additional note
 You may see and output similar to the following. Primarily showing that the ansible controller can reach its target hosts:
***
[DEPRECATION WARNING]: Ansible will require Python 3.8 or newer on the
controller starting with Ansible 2.12. Current version: 2.7.18 (default, Jun 10
 2021, 00:11:02) [GCC 7.3.1 20180712 (Red Hat 7.3.1-13)]. This feature will be
removed from ansible-core in version 2.12. Deprecation warnings can be disabled
 by setting deprecation_warnings=False in ansible.cfg.
[WARNING]: Platform linux on host localhost is using the discovered Python
interpreter at /usr/bin/python, but future installation of another Python
interpreter could change the meaning of that path. See https://docs.ansible.com
/ansible/2.11/reference_appendices/interpreter_discovery.html for more
information.
localhost | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}


[WARNING]: Platform linux on host docker_host is using the discovered Python
interpreter at /usr/bin/python, but future installation of another Python
interpreter could change the meaning of that path. See https://docs.ansible.com
/ansible/2.11/reference_appendices/interpreter_discovery.html for more
information.
docker_host | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
***
