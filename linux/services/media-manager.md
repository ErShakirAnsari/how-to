```
[Unit]
Description=Media Manager rest api in SpringBoot
After=syslog.target

[Service]
User=ao
Group=ao
ExecStart=java -jar /opt/ajaxer-org/springboot/media-manager.jar --spring.config.location="file:/opt/ajaxer-org/springboot/application.yml"
SuccessExitStatus=143
Restart=always
RestartSec=30

# stop log to in /var/log/syslog
StandardOutput=null
StandardError=null
#StandardOutput=append:/var/log/media-manager.out.log
#StandardError=append:/var/log/media-manager.err.log

[Install]
WantedBy=multi-user.target
```