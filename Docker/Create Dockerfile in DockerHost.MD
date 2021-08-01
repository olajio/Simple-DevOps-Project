# Create Dockerfile in DockerHost

#### Create Dockerfile as follows at the home directory of the dockeradmin user
```
FROM tomcat:8.0

MAINTAINER O Olajide

COPY ./webapp.war /usr/local/tomcat/webapps
```


#### Create an image with the Dockerfile
`docker build -t devops-project .`


#### Create a container with the image
`docker run -d --name devops-container -p 8080:8080 devops-project`


#### Access the application
`http://PUBLIC_IP:8080/webapp/`
