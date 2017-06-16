# FUSE Docker image

This project builds a Docker image for [JBoss Fuse](http://www.jboss.org/products/fuse/overview/).

Based on https://github.com/jboss-fuse/jboss-fuse-docker

## Usage


### With local nexus
When running a nexus container (https://github.com/sonatype/docker-nexus):

Start nexus (e.g, using the volume container approach for persistence)

    docker volume create --name nexus-data
    docker run -d -p 8081:8081 --name nexus3 -v nexus-data:/nexus-data  sonatype/nexus3
    
Fuse connects to `snapshots` and `releases` maven repos (**make sure you configure these in nexus**) 

Then run fuse:

    docker run -Pd --name fuse --link nexus3  vaida/clip-fuse

I have enabled the admin=admin user


## Using the image


The administration console should be available at [http://localhost:8181/hawtio](http://localhost:8181/hawtio)

ssh access to karaf console at `ssh -p <port> admin@localhost`

## Ports Opened by Fuse

You may need to map ports opened by the Fuse container to host ports if you need to access it's services.
Those ports are:

* 8181 - Web access (also hosts the Fuse admin console).
* 8101 - SSH Karaf console access
* 61616 - AMQ Openwire port.

## Image internals

This image extends the [`jboss/base-jdk:8`](https://github.com/JBoss-Dockerfiles/base-jdk/tree/jdk8) image which adds the OpenJDK distribution on top of the [`jboss/base`](https://github.com/JBoss-Dockerfiles/base) image. Please refer to the README.md for selected images for more info.

The server is run as the `jboss` user which has the uid/gid set to `1000`.

Fuse is installed in the `/opt/jboss/jboss-fuse` directory.