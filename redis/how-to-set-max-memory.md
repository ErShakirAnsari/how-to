## How to set max memory

#### Step1 ➖ Create a file /path/file.conf and put below two lines

```
maxmemory 128mb
maxmemory-policy allkeys-lru
```

💡 Notes

- /home/user/file.conf may not work, try put it into different folder.

- Units: when memory size is needed, it is possible to specify  
  it in the usual form of 1k 5GB 4M and so forth:

| unit | = | value                    |
|------|---|--------------------------|
| 1k   | = | 1000 bytes               |
| 1kb  | = | 1024 bytes               |
| 1m   | = | 1000000 bytes            |  
| 1mb  | = | 1024*1024 bytes          |
| 1g   | = | 1000000000 bytes         |
| 1gb  | = | 1024 * 1024 * 1024 bytes |

__Units are case insensitive so 1GB 1Gb 1gB are all the same.__

> LRU means __Least Recently Used__  
> LFU means __Least Frequently Used__

#### Step2 ➖ Add an include tag in /etc/redis/redis.conf file

```
include /path/file.conf
```

#### Step3 ➖ Restart redis server

```
sudo service redis-server restart
```

OR

```
sudo systemctl restart redis
```

OR

```
 sudo systemctl restart redis-server
```

### 🔗 Reference

- [Redis Docs](https://redis.io/docs/management/config-file/)
- [stackoverflow](https://stackoverflow.com/a/59465061/7418534)
