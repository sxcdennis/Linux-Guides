# Install and Configure VNC-Server

VNC (Virtual Network Computing) is a server-client protocol which allows user accounts to remotely connect and control a distant system by using the resources provided by the Graphical User Interface.
We will be using **tingervnc-server**

## Installing

**1.** Install tigervnc-server
`yum install -y tigervnc-server`

**2.** Set a password for vncserver. You can change to whatever user you want

```
#You can change to wahtever user you want or use root
vncpasswd

```


# Configuring files

**1.** Copy configuration file.

`cp /lib/systemd/system/vncserver@.service  /etc/systemd/system/vncserver@:1.service`

**2.** Edit new file

`vi /etc/systemd/system/vncserver@\:1.service`

The value of 1 after @ sign represents the display number (port 5900+display). Also, for each started VNC server, the port 5900 will be incremented by 1.

**3.** Add the following lines to file
```
[Unit]
Description=Remote desktop service (VNC)
After=syslog.target network.target

[Service]
Type=forking
ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'
ExecStart=/sbin/runuser -l my_user -c "/usr/bin/vncserver %i -geometry 1280x1024"
PIDFile=/home/my_user/.vnc/%H%i.pid
ExecStop=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'

[Install]
WantedBy=multi-user.target

```
