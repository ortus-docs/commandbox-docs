# Code Endpoints

For CommandBox to be able to install packages for you it needs to connect to code repository where packages are stored so it can download them for installation. CommandBox integrates seamlessly with ForgeBox, our community of ColdFusion (CFML) projects. CommandBox will soon be able to integrate with git, svn, http, ftp and custom code endpoints.

Signing up for a ForgeBox account is quick, easy, and free. You will need your own account to post code, but anyone can browse and install packages. The web UI for ForgeBox is currently located at http://www.coldbox.org/forgebox.

Inside CommandBox, use the **forgebox** namespace to **search** for packages or **show** packages of your choosing.

```
forgebox show recent modules
forgebox show coldbox
forgebox search REST
```

For companies who want to host internal code endpoints for private packages, we will soon support an Enterprise version of ForgeBox that can be installed behind your company's firewall. Please contact us if this feature interests you.