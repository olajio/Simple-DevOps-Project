# Installing Docker on Amazon Linux server

### Pre-requisites
1. Amazon Linux EC2 Instance

## Installation Steps

1. Install docker and start docker services
   ```sh 
   yum install docker -y
   docker --version 
   
   # start docker services
   service docker start
   service docker status
   ```
1. Create a user called dockeradmin
   ```sh
   useradd dockeradmin
   passwd dockeradmin
   ```
1. add a user to docker group to manage docker 
   ```
   usermod -aG docker dockeradmin
   ```
   
## Alternative Docker Installation Steps - using user data with Amazon AMI:

1. Use User data during deployment of EC2 to automatically in docker

Rather than following the steps above to install docker after deploying the Amazon linux AMI, I rather used the following user data to hasting the steps:
```
#!/bin/bash
yum update -y
amazon-linux-extras install docker -y
yum install docker -y
systemctl start docker
systemctl enable docker
useradd dockeradmin
echo dockeradmin | passwd dockeradmin --stdin
usermod -aG docker dockeradmin
usermod -aG docker ec2-user    #this line is not required. just adding it as I may want to run docker commands with my ec2-user
echo "docker_host" > /etc/hostname
PRIVATE_IP=$(hostname -I | awk '{print $1}')
echo "$PRIVATE_IP docker_host" >> /etc/hosts
reboot
```
### Validation test
1. Create a tomcat docker container by pulling a docker image from the public docker registry
   ```sh
   docker run -d --name test-tomcat-server -p 8090:8080 tomcat:latest
   ```

## Docker Installation on CentOS server
##### Referance URL : https://docs.docker.com/install/linux/docker-ce/centos/#install-using-the-repository
### Pre-requisites

Please follow below steps to install docker CE on CentoOS server instance. For RedHat only Docker EE available 

1. Install the required packages.

   ```sh 
   sudo yum install -y yum-utils \
   device-mapper-persistent-data \
   lvm2
   ```
  
1. Use the following command to set up the stable repository.
 
   ```sh 
   sudo yum-config-manager \
   --add-repo \
   https://download.docker.com/linux/centos/docker-ce.repo
   ```

### INSTALLING DOCKER CE

1. Install the latest version of Docker CE.
   ```sh 
   sudo yum install docker-ce
   ```

   Note: If prompted to accept the GPG key, verify that the fingerprint matches 
060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35, and if so, accept it.

1. Start Docker.
   ```sh 
   sudo systemctl start docker
   ```

1. Verify that docker is installed correctly by running the hello-world image.
   ```sh
   sudo docker run hello-world
   ```
