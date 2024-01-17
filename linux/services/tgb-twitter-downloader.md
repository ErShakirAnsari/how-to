```
[Unit]
Description=Telegram bot for simple media downloader
After=syslog.target

[Service]
User=ao
Group=ao
ExecStart= java -jar /opt/ajaxer-org/telegram/tgb-twitter-downloader.jar
SuccessExitStatus=143
Restart=always
RestartSec=30

# stop log to in /var/log/syslog
StandardOutput=null
StandardError=null
#StandardOutput=append:/var/log/tgb-twitter-downloader.out.log
#StandardError=append:/var/log/tgb-twitter-downloader.err.log

[Install]
WantedBy=multi-user.target
```