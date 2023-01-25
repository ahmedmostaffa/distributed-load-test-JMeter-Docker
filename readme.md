


# Documentation
![docker-jmeter](https://user-images.githubusercontent.com/94404894/214570631-2fb96de0-910c-4679-b4e7-354f763e882d.png)


As you see in the above JMeter architecture (master, slaves), through this documentation we will learn how to implement this architecture or(how to build your infrastructure using docker) into JMeter using docker to do distributed load testing using docker.
## Steps
* 1-  ensure the docker engine is successfully installed on your local machine
* 2- Run the below commands consecutively.
```Docker
docker run -dit --name slave01 vinsdocker/jmserver /bin/bash
docker run -dit --name slave02 vinsdocker/jmserver /bin/bash
docker run -dit --name slave03 vinsdocker/jmserver /bin/bash
.
.
```
The docker engine will pull the images from docker hub and create an ***n*** container based on your test design if you wanna do load testing with 3 concurrent users so you should write the above commands 3 times and so on.

* 3- Run the below command to create a container for JMeter master. 
``` Docker
docker run -dit --name master vinsdocker/jmmaster /bin/bash
```
* 4- To check if all containers are running successfully, type the below command.

```
docker ps -a
```
* 5- Get the list of IP addresses for the running containers via this command
```
docker inspect --format '{{ .Name }} => {{ .NetworkSettings.IPAddress }}' $(sudo docker ps -a -q)
```

the result will be something like the following:
```
IP address for master/container `172.17.0.2`
IP address for slave01/container `172.17.0.3`
IP address for slave02/container `172.17.0.4`
IP address for slave03/container `172.17.0.5`
```
* 6- Copy any files from the local machine to the master container, you can copy your JMeter test script through this command. For ex: 
```Docker
docker exec -i master sh -c "cat > /jmeter/apache-jmeter-5.5/bin/JMeter-test-script.jmx" < JMeter-test-script.jmx
```
* 7- Go inside your running master container.
```
docker exec -it master /bin/bash
```
* 8- finally, let's run the test script in master container with passing the IP addresses of the running slaves.
``` 
jmeter -n -t /jmeter/apache-jmeter-3.3/bin/JMeter-test-script.jmx -l report/results.csv -f -e -o report/html -R172.17.0.3,172.17.0.4,172.17.0.5,172.17.0.6,172.17.0.7,172.17.0.8,172.17.0.9,172.17.0.10,172.17.0.11,172.17.0.12
```
## Exporting HTML, CSV Report.
* at the end of the blog you can read and access html, csv reports at your local machine through the below command.

```
docker cp master:/path/report/* ~/Desktop/reporting
```




