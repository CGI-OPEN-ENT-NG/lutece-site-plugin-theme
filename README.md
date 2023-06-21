
# Plugin Theme CGI

## Introduction

This plugin lets manage different graphical themes for the site.


[Maven documentation and reports](http://dev.lutece.paris.fr/plugins/plugin-theme/)


## Development (standard)

### **Prerequisites**

-   Apache Maven v3.5.4
-   Apache Tomcat v9.0.73
-   Java OpenJDK v8
-   Docker v24.0.2

### Build and deploy

#### Build your app

Run a build of the application

```bash
mvn clean install -U
```

#### Development

To do this, modify the server.xml file in your Tomcat/conf folder, adding a Context tag :

```text
<Host ...>
    ...
    <Context docBase="(...)/myapp/target/<exploded_directory>" path="/myapp" workDir="(...)/myapp/target/<exploded_directory>/work" />
</Host>
```

More info [here](https://lutece.paris.fr/support/wiki/install-lutece-application.html)

##### Production

For example, in /etc/systemd/system/tomcat.service
```text
[Unit]
Description=Tomcat webs servlet container
After=network.target

[Service]
Type=forking

User=tomcat
Group=tomcat

Environment="JAVA_HOME=/usr/lib/jvm/jre"
Environment="JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom"

Environment="CATALINA_BASE=/appli/tomcat"
Environment="CATALINA_HOME=/appli/tomcat"
Environment="CATALINA_PID=/appli/tomcat/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"

ExecStart=/appli/tomcat/bin/startup.sh
ExecStop=/appli/tomcat/bin/shutdown.sh

[Install]
WantedBy=multi-user.target
```

##### Finally

Start tomcat server

```bash
systemctl restart tomcat 
```

#### Deploy your app

Performing the deployment process is a simple task:

```bash
docker run -it --rm -h maven -w /usr/src/mymaven -v "$PWD:/usr/src/mymaven" -e MAVEN_CONFIG=/var/maven/.m2 -e cgi_username=XXXXX -e cgi_password=XXXXX maven:3.5.4-alpine mvn clean deploy -Dmaven.test.skip=true
```

Skipping tests is OK in the context of a deployment job because this job should be the last job from a deployment pipeline for the project.

A common example of such a deployment pipeline would be a succession of Jenkins jobs, each triggering the next only if it completes successfully. As such, it is the responsibility of the previous jobs in the pipeline to run all tests suites from the project – by the time the deployment job runs, all tests should already pass.


