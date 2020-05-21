# Creating Spring Boot Admin Docker Image
This repository is used for creating a docker image running Codecentric's spring-boot-admin, see https://github.com/codecentric/spring-boot-admin.
Images of built by this project are published in https://hub.docker.com/r/michayaak/spring-boot-admin with the image version reflecting SBA's version.

## Prerequisites:
* Java 11 (or newer) 
* Maven

## Building a docker image with the Spring Boot Admin Application and pushing to hub.docker.com
From the root of the repository, run:
```shell script
export SBA_DOCKER_REPO=michayaak/spring-boot-admin
mvn clean install dockerfile:build dockerfile:tag@tag-version dockerfile:push@push-latest dockerfile:push@push-version
```

# Running the Spring Boot Admin Container
```shell script
docker run -p 8082:8082 michayaak/spring-boot-admin
```

Spring Boot applications should register with SBA using an IP address that can be accessed from the container.
Note that If your application isn't running in a container and it binds to `localhost`/`127.0.0.1` (as opposed to `0.0.0.0`)  it should register with SBA as `http://host.docker.internal/actuator` so the SBA container could access it the actuator API.

## SBA Versioning
The tags of the images in this repository are the target SBA version, so running SBA version 2.0.0 you should use `michayaak/spring-boot-admin:2.0.0`.

# Customizing Spring Boot Admin in a Container
When running in a container, customizing Spring Boot Admin is done by putting the configuration files in a directory that is mounted as `/config` inside the container which is than read by Spring Boot.

For example, to enable email notifications with a custom template, create a `config` directory on the docker host with the following `application.yml`:
```yml
spring:
  boot:
    admin:
      notify:
        mail:
          enabled: true
          additional-properties:
            environment: Heaven
          to: my@mail.com
          template: file:config/email-notification-template.html
.status}*'
  mail:
    host: mail.com
```
Next add to this directory a file `email-notification-template.html` with your custom template.

Now start Spring Boot Admin using the following command:
```shell script
docker run --name sba --rm -p 8082:8082 -v $PWD/config:/config michayaak/spring-boot-admin
```

Spring Boot will read the `application.yml` and SBA will be configured accordingly. 