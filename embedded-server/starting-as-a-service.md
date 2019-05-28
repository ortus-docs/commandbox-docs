# Starting as a Service

When using CommandBox on a staging or production server, you may wish to start up servers as a service when the OS comes online. Here is a guide on each major OS type.

## Windows

See screencast here: https://www.ortussolutions.com/blog/screencast-starting-commandbox-servers-as-a-windows-service

## MacOS

Coming soon...

## Ubuntu \(init.d\)

Coming soon...

## CentOS/RHEL \(system.d\)

Create a `.service` file

```text
nano /usr/lib/systemd/system/mySite.service
```

as follows:

```text
[Unit]
Description=mySite Service

[Service]
ExecStart=/usr/bin/box server start /var/www/mySiteAPI/server.json
Type=forking

[Install]
WantedBy=multi-user.target
```

Start the service

```text
systemctl start mySite.service
```

Give the service about a minute to load up, then check its status

```text
systemctl status mySite.service
```

Once you've verified the service is running as expected, enable the service to load at boot

```text
systemctl enable mySite.service
```

