## `tigervnc` tutorial

### commands
- `vncserver`
  - `vncserver -list`: list existed vnc servers
  - `vncserver -kill :portnumber` kill a specified vnc server
  - `vncserver -kill :*`: kill all existed vnc server (started by you)
  - `vncserver -geometry 1920x1000 :1`: start a vnc server and set the size to 1920x1000
  - `vncserver -localhost yes :1`: start a vnc server which accepts connections only from localhost (default)
  - `vncserver -localhost no :1`: start a vnc server which accepts connections from out

- `vncpasswd`:
  - set your vnc password

- `vncconfig`:
  - configure vnc

### a simple guide:
```
vncserver :1
vncserver -kill :1
cp /share/softwares/xstartup ~/.vnc/xstartup
chmod +x ~/.vnc/xstartup
vncserver -geometry 1920x1000 -localhost no :1
```
Remember to change the port number, valid values are 1-100 (for internal visit), 1-20 (for external visit).


### my `~/.vnc/xstartup` file:

for Xfce desktop:
```
#!/bin/sh
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
startxfce4 &
```

for KDE desktop:
```
#!/bin/sh
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
dbus-launch startxfce4 &
```
