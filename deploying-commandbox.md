![](/assets/docker_logo.png)# Deploying Production CFML Servers with CommandBox

CommandBox may also be used in production deployments, since it allows you to orchestrate your server environment.  This eliminates dependency on hardware, and makes your CFML applications more portable, as a whole.   For advanced server configurations, be sure to check out the [`CFConfig` module](https://cfconfig.ortusbooks.com/).

Since the startup of a CommandBox server allows you specify a host and server port, you can easily bind your server to a machine IP address and specify which port it should serve the application on. This allows you to proxy traffic to the application from IIS, Apache, or NGINX and even allows you to serve traffic directly on HTTP port 80 or 443, if you choose.

Container-based deployments are also supported, with official Docker Images and a buildpack for Heroku/Dokku.




[![Docker Logo](/images/docker_logo.png)](/deploying-commandbox/docker.md)

[_Click to learn more._](/deploying-commandbox/docker.md)



[![Heroku Logo](/images/heroku_logo.png)](/deploying-commandbox/heroku-buildpack.md)

[_Click to learn more._](/deploying-commandbox/heroku-buildpack.md)






