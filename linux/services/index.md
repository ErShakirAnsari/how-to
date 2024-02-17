# Services

### How to enable/disable a service, so it can be auto start on system reboot;

**1) Enable/Disable the service**

```
sudo systemctl enable httpd
```

OR

```
sudo systemctl disable httpd
```

**2) Reload the demon**

```
sudo systemctl daemon-reload
```

**3) Check the status**

```
sudo systemctl is-enabled httpd
```

[Referenece](https://serverfault.com/a/861935)

### Other services example:

- [media-manager](./media-manager.md)
- [tgb-twitter-downloader](./tgb-twitter-downloader.md)
- [tomcat9](./tomcat9.md)
