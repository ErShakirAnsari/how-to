```
[Unit]
Description=Tomcat auto startup script
After=syslog.target

[Service]
Type=forking
User=ao
Group=ao

Environment="JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-arm64"
Environment="JAVA_OPTS=-Djava.security.egd=file:///dev/urandom -Djava.awt.headless=true"

ExecStart=/opt/ajaxer-org/tomcat9/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

SuccessExitStatus=143
Restart=always
RestartSec=30

[Install]
WantedBy=multi-user.target
```