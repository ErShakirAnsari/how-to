# Some useful SSH commands

#### How to make local connection alive?

Add below lines in `~/ssh/.config` file

```
Host *
    ServerAliveInterval 20
    TCPKeepAlive no
```