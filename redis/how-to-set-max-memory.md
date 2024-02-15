## How to set max memory

#### Step1 ➖ Create a file /path/file.conf and put below two lines

```
maxmemory 128mb
maxmemory-policy allkeys-lru
```

OR

```
maxmemory 6gb
maxmemory-policy allkeys-lru
```

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

