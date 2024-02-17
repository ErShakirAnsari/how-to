# Services

### How to enable a service so it can be auto start on system reboot;

**1) Enable the service**

```
sudo systemctl enable httpd
```

**2) Reload the demon**

```
sudo systemctl daemon-reload
```

**3) Start the service**

```
sudo systemctl start httpd
```

**3) Check the status**

```
sudo systemctl status httpd
```

[Referenece](https://serverfault.com/a/861935)

### Other services example:

- [media-manager](./media-manager.md)
- [tgb-twitter-downloader](./tgb-twitter-downloader.md)
- [tomcat9](./tomcat9.md)
