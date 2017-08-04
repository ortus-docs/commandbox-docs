# Using FusionReactor with your servers
FusionReactor is a popular tool for monitoring performance and gathering metrics for ColdFusion and Java J2EE applications.  You may with to use FusionReactor with the servers you start up via CommandBox.  We've created a CommandBox module that will add FusionReactor support to your CommandBox servers which you can view here on ForgeBox.

https://www.forgebox.io/view/commandbox-fusionreactor

## Installation

The CommandBox FusionReactor module is a separate project that you can optionally install by typing this:
```
install commandbox-fusionreactor
```

That's it-- now every server you start with the `start` command will automatically have the JVM args added to it to load up FusionReactor.  

## Usage 

There's nothing special you have to do in order for FusionReactor to load.  A random port will be chosen  for FusionReactor to use so you can have more than one server running at a time and each of them will have their own FusionReactor instance running.   When you start a server, you should see some output similar to this:

```
******************************************
* CommandBox FusionReactor Module Loaded *
******************************************

FusionReactor will be available at the URL http://127.0.0.1:2871
```

To open FusionReactor in your browser, you can run the following command:
```
fr open
```
If you right click on the tray icon for this server, you'll see there is a new menu item at the bottom called `Open Fusion Reactor` that will do the same thing.

## Licensing
FusionReactor is a commercial product and requires a license to use.  If your company has a license for you to use on your PC, then you can register your CommandBox FusionReactor licence with this command:
```
fr register "myLicenseKey"
```
If you don't have a license, you can sign up for a trial and purchase a license from the [http://www.fusion-reactor.com/](http://www.fusion-reactor.com/?affiliate=ortussolutions) website.

## Custom Port

By default, the module always picks a random port to start Fusionreactor on.  You can also set your FusionReactor port on a per-server basis, or at a global level for all servers (if using the fancy [host updater module](https://www.forgebox.io//view/commandbox-hostupdater) which prevents port conflicts by binding each site to its own IP). The default behavior will still be to pick a random port if you don't specify one.

```
// Server specific port
server set fusionreactor.port=8088
// global default port
config set server.defaults.fusionreactor.port=8088
```

## Disable the module
You may want to turn the FusionReactor functionality on or off based on your testing or for specific sites.  There is now an enable flag for just that.  It can be set per server and for all servers as well.

```
server set fusionreactor.enable=false
config set server.defaults.fusionreactor.enable=false
```

## Per-server License Key

You can set a license key per server if you wish like so:
```
server set fusionreactor.licenseKey=XXXXX-XXXXX-XXXXX-XXXXX-XXXXX
```

## Managing Versions

The module is regularly updated to use the latest version of FusionReactor.  Note however that your license key may not be for the latest FR version.  When the internal default version of FR is updated for a major release, the version of the actual FR module will also have a "major" version increment.  This is so you can always run `upgrade --system` and you won't have to worry about suddenly getting a major FR upgrade one day that doesn't work with your license key.

If you want to upgrade your CommandBox FusionReactor module to a new major release, just re-run the installation command.

```
install commandbox-fusionreactor
```

## Custom Version of FusionReactor

If you have an older FR license you want to use, you can specify the version of FR you'd like like so:

```
// Server specific version
server set fusionreactor.version=6.2.8
// global default version
config set server.defaults.fusionreactor.version=6.2.8
```

## Custom Download URL
If you have a specific version of the FusionReactor jar that you want to use, or you need to cache it internally in a local network, you can even override the download URL for the jar file.  This URL is only hit once and then the jar is cached.  And of course, this can be set globally or for a single server so you can test more than one version of FusionReactor at a time.  

If one server is downloading a custom URL, make sure you also set a unique jarPath setting as well so it doesn't interfere with your others servers still using the default download.
```
server set fusionreactor.downloadURL=http://site.com/custom/path/fusionreactor.jar
server set fusionreactor.jarPath=/FR-home/fusionreactor-custom.jar
```
Or override it for all your servers:
```
config set server.defaults.fusionreactor.downloadURL=http://site.com/custom/path/fusionreactor.jar
config set server.defaults.fusionreactor.jarPath=/FR-home/fusionreactor-custom.jar
```



