Prereq: [Docker_Installation_Steps.MD](https://github.com/olajio/Simple-DevOps-Project/blob/master/Docker/Docker_Installation_Steps.MD)

# Integrate DockerHost with Jenkins

To integrate DockerHost to Jenkins, you would install `Publish over SSH` plugin as follows:


`Manage Jenkins` --> `Configure System` --> Under `Publish over SSH` section ---> Click the `Add` button under `SSH Servers` and fill-out the form as follows

<img width="1531" alt="Screen Shot 2021-07-31 at 6 55 52 PM" src="https://user-images.githubusercontent.com/34173234/127754982-05aae2d6-892c-499d-926b-9ecf6e182eb0.png">

Click `Advanced` button. To enter the passowrd, check the `Use password authentication, or use a different key` option and fill-out the password and test the connection, and continue filling the form with the with the password details and click `Test Configuration`

<img width="1546" alt="Screen Shot 2021-07-31 at 7 01 38 PM" src="https://user-images.githubusercontent.com/34173234/127755062-46a6f468-b826-425b-afcd-ea3d228f08e7.png">


##### Note: If the connection failed, it may be because EC2 in blocks password authentication by default. To address this, edit the `/etc/ssh/sshd_config` file and look for the `PasswordAuthentication` line to enable password authentication as follows: `PasswordAuthentication yes`. Then restart SSH service: `systemctl restart sshd`



Next: [Jenkins job to copy artifacts to DockerHost](https://github.com/olajio/Simple-DevOps-Project/blob/master/Docker/Jenkins%20job%20to%20copy%20artifacts%20to%20DockerHost.MD)
