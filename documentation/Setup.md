---
layout: plain
title: Setup
---

The easiest way to setup Polypheny-DB is to use [Polypheny Control](https://github.com/polypheny/Polypheny-Control). 


### Requirements
To build and start Polypheny-DB using Polypheny Control you need to have a Java JDK of version 11 or higher installed on your system.
Thanks to [JGit](https://github.com/eclipse/jgit), Polypheny Control contains a pure Java implementation of Git. Therefore, it is no longer required to have Git installed on the system.


### Setup
Download the latest [polypheny-control.jar](https://github.com/polypheny/Polypheny-Control/releases/latest) from the release section. 
To start the Web-UI execute `polypheny-control.jar` by specifying the parameter `control`.

```java
java -jar polypheny-control.jar control
```

The interface can now be accessed on port `8070`. This port can be changed by specifying another port using the parameter `-p`:

```java
java -jar polypheny-control.jar control -p 8070
```

We strongly recommend not to use port `8080`, `8081` and `8082` because these are the default ports of services offered by Polypheny-DB.


### Data Folder
Polypheny-DB stores files in the local file system. By default, this is done in a special folder called `.polypheny` in the home directory of the executing user.
This location can be customized by setting a system environment variable called `POLYPHENY_HOME` pointing to the desired location before Polypheny-DB is started.


### (Optional) Setup Docker
See [this](Stores/Docker.md) on how to setup Docker to use the built-in Docker-based data store deployment.
