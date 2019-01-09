# Starting as a Service

When using CommandBox on a staging or production server, you may wish to start up servers as a service when the OS comes online.  Here is a guide on each major OS type.

## Windows

Coming soon...

## MacOS

Coming soon...

## Ubuntu \(init.d\)

Coming soon...

## CentOS/RHEL \(system.d\)

Create a `.service` file 
```
nano /usr/lib/systemd/system/coldbox.service
```
as follows:
```
[Unit]
Description=ColdBox Service

[Service]
ExecStart=/usr/bin/box server start /var/www/coldboxAPI/server.json
Type=forking

[Install]
WantedBy=multi-user.target
```

Start the service
```
systemctl start coldbox.service
```

Give the service about a minute to load up, then check its status
```
systemctl status coldbox.service
```

Once you've verified the service is running as expected, enable the service to load at boot
```
systemctl enable coldbox.service
```


