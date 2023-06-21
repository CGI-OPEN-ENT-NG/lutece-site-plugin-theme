
# Plugin Theme CGI

## Introduction

This plugin lets manage different graphical themes for the site.


[Maven documentation and reports](http://dev.lutece.paris.fr/plugins/plugin-theme/)


## Development (standard)

### **Prerequisites**

-   Apache Maven v3.5.4
-   Apache Tomcat v9.0.73
-   Java OpenJDK v8

### Build

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

### Finally

Start tomcat server

```bash
systemctl restart tomcat 
```
