# Creating Spring Boot Admin Docker Image
This repository is used for creating a docker image running spring-boot-admin.
Images of built by this project are published in https://hub.docker.com/r/michayaak/spring-boot-admin with the image version reflecting SBA's version.

## Building a docker image with the Spring Boot Admin Application
Prerequisites:
* Java 8 (or newer) 
* Maven

From the root of the repository, run:
When creating the docker image, specify the version of SBA in the SBA_VERSION environment variable.
```shell script
export SBA_VERSION=2.2.2
export SBA_DOCKER_REPO=michayaak/spring-boot-admin
mvn install dockerfile:build dockerfile:tag@tag-version
```

## Publishing to hub.docker.io
When pushing to https://hub.docker.com, specify the target repository need to be set in the SBA_DOCKER_REPO environment variable.
```shell script
export SBA_VERSION=2.2.2
export SBA_DOCKER_REPO=michayaak/spring-boot-admin
mvn dockerfile:push@push-latest dockerfile:push@push-version
```

# Running the Spring Boot Admin Container
```shell script
docker run -p 8082:8082 michayaak/spring-boot-admin
```