# how-to-debug-in-android-studio-using-adb-over-wifi

### 1) Connect your phone with USB, make sure usb debugging is enabled.

### 2) Open command/terminal and `cd` to your `platform-tools`

i.e

```
cd C:\Users\username\AppData\Local\Android\Sdk\platform-tools
```

### 3) Start the adb server

```
adb start-server
```

💡 if you face any issue while starting, try to kill previous process and then try start.

```
adb kill-server
```

### 4) Connect adb via tcp `adb tcpip <port>`

```
adb tcpip 5555
```

💡 if port already in use, pl use different port

### 5) find out phone's IP address

`Settings -> About phone -> Status` (some phones may be vary)  
💡 In case that you connect more than one device, disconnect other phones except the one you need to connect.