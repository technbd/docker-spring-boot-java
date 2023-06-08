Docker Container With Spring Boot Web App Demo
==================================================
The aim is to demonstrate a running docker container with a spring boot standalone web service thus, I've written the web service with Spring Boot as simple.


Dockerfile
-----------
With the **Dockerfile** we can define, configure and initialize our image and container. In **Dockerfile** we can define;

1. Which image we are going to use. We need a docker image that has the OpenJDK and thus we are going to use Alpine;

```
    #FROM openjdk:8-jre-alpine
    FROM openjdk:17-alpine
```

2. Which shell we are going to have. I prefer **Bash** which I find easy to use; # set shell to bash # source: https://stackoverflow.com/a/40944512/3128926

```
    RUN apk update && apk add bash
```

3. The working directory our app is going to run. The name of the working directory will be **app**;

```
    WORKDIR /app
```

4. The files we need to copy into our image. We will need only the **Uber Jar** file so let's copy it into the working dir;

```
    COPY /target/*.jar app.jar
```

5. The port number(s) that we need to expose to reach out from the container. Spring Boot default port is 8080;

```
    EXPOSE 8080
```

6. The commands that we need to run as the container goes live. And we need to add simply "java -jar <jar_file>.jar" format;

```
    CMD ["java", "-jar", "app.jar"]
```

Notify that the CMD(command) parameters are separated with comma.


Building The Image
------------------
Before building the image, we need to create the **Uber Jar** (aka **Fat Jar**) file, to do so, just clean install with maven as below;

```
    mvn clean install
```

Now our **Jar** file is created under the target folder with the format: "target/<final_name>.jar" (or .war based on package in .pom file)

To build the image, we will use **docker build** command and tag it. The last parameter will be the directory, by using dot ("."),
we point to the current directory. So you must run this command on the top level directory of this repository.

```
    docker build -t <docker_username>/<image_name> .
```

As you run the command which is written above, a successful message.



After the image is built, you can check it with the 
#docker image ls command. 



Running And Testing The Docker Container
------------------------------------------
Now we have our docker image ready however, we need to run a container based on our image.

The format is simple;

```
docker run -dit -p <external-port>:<internal-port> <image-name>
```

We can simply run our docker container
as below;

```
docker run -dit -p 80:8080 <docker_username>/<image_name>
```

The parameter **p** stands for the **port**. **External port** is the port that will be available outside of the docker container.
The **internal port** is the port that will be available inside the docker container, which is **8080** because the default port of
Spring Boot is set to **8080**. So outside of the Docker Container, we will use the port number **80**, and it is mapped to **8080**
and will reach to the Spring Boot application.

Checking Docker Container
-------------------------
Check running containers with the following command;

```
docker container ls
```

