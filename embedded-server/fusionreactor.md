# FusionReactor

FusionReactor is a popular tool for monitoring performance and gathering metrics for ColdFusion and Java J2EE applications. You may wish to use FusionReactor with the servers you start up via CommandBox. We've created a CommandBox module that will add FusionReactor support to your CommandBox servers which you can view here on ForgeBox.

[https://www.forgebox.io/view/commandbox-fusionreactor](https://www.forgebox.io/view/commandbox-fusionreactor)

## Installation

The CommandBox FusionReactor module is a separate project that you can optionally install by typing this:

```text
install commandbox-fusionreactor
```

That's it-- now every server you start with the `start` command will automatically have the JVM args added to it to load up FusionReactor.

## Usage

There's nothing special you have to do in order for FusionReactor to load. A random port will be chosen for FusionReactor to use so you can have more than one server running at a time and each of them will have their own FusionReactor instance running. When you start a server, you should see some output similar to this:

```text
******************************************
* CommandBox FusionReactor Module Loaded *
******************************************

FusionReactor will be available at the URL http://127.0.0.1:2871
```

To open FusionReactor in your browser, you can run the following command:

```text
fr open
```

If you right click on the tray icon for this server, you'll see there is a new menu item at the bottom called `Open Fusion Reactor` that will do the same thing.

## Licensing

FusionReactor is a commercial product and requires a license to use. If your company has a license for you to use on your PC, then you can register your CommandBox FusionReactor license with this command:

```text
fr register "myLicenseKey"
```

If you don't have a license, you can sign up for a trial and purchase a license from the [http://www.fusion-reactor.com/](http://www.fusion-reactor.com/?affiliate=ortussolutions) website.

### Hide License Key

By default, the license key will always be displayed on the home page after login. You can turn off the display of the key on a per-server basis or at a global level for all servers.

```text
// Server specific
server set fusionreactor.hideLicenseKey=true
// global default
config set server.defaults.fusionreactor.hideLicenseKey=true
```

## Custom Port

By default, the module always picks a random port to start FusionReactor on. You can also set your FusionReactor port on a per-server basis, or at a global level for all servers \(if using the fancy [host updater module](https://www.forgebox.io//view/commandbox-hostupdater) which prevents port conflicts by binding each site to its own IP\). The default behavior will still be to pick a random port if you don't specify one.

```text
// Server specific port
server set fusionreactor.port=8088
// global default port
config set server.defaults.fusionreactor.port=8088
```

FusionReactor will bind the port on whatever host address is used for your server.

## Disable the module

You may want to turn the FusionReactor functionality on or off based on your testing or for specific sites. There is now an enable flag for just that. It can be set per server and for all servers as well.

```text
server set fusionreactor.enable=false
config set server.defaults.fusionreactor.enable=false
```

## Per-server License Key

You can set a license key per server if you wish like so:

```text
server set fusionreactor.licenseKey=XXXXX-XXXXX-XXXXX-XXXXX-XXXXX
```

## Managing Versions

The module is regularly updated to use the latest version of FusionReactor. Note however that your license key may not be for the latest FR version. When the internal default version of FR is updated for a major release, the version of the actual FR module will also have a "major" version increment. This is so you can always run `upgrade --system` and you won't have to worry about suddenly getting a major FR upgrade one day that doesn't work with your license key.

If you want to upgrade your CommandBox FusionReactor module to a new major release, just re-run the installation command.

```text
install commandbox-fusionreactor
```

## Custom Version of FusionReactor

If you have an older FR license you want to use, you can specify the version of FR you'd like like so:

```text
// Server specific version
server set fusionreactor.installID=fusionreactor@7.x
// global default version
config set server.defaults.fusionreactor.installID=fusionreactor@7.x
```

The `installID` setting can be any valid CommandBox endpoint installation ID, which means you can point to a custom HTTP URL, or ForgeBox slug, etc.

## Debugger Libs

As of version 4.0 of this module, the debugger libs will be added automatically for you based on your OS.  To disable the debugger libs use the following setting:

```bash
// Server specific version
server set fusionreactor.debugEnable=false
// global default version
config set server.defaults.fusionreactor.debugEnable=false

```

## Provide External reactor.conf File

There are a handful of JVM args we can use to set things like license key or password, but there are many many settings inside of FusionReactor that have no corresponding method to externalize them.  These are stored in a file called `conf/reactor.conf` inside of the FusionReactor home directory.  If you want to script out settings such as 

* E-mail servers
* Notification settings
* Profiler settings
* Request history settings

Then you can make a copy of a `reactor.conf` file that contains the settings you want the point this module at the file to be copied over when starting the server so FR will pick it up and use it.  Just need in mind that this setting will **override** any manual setting changes you make in the FR web admin every time you start the server.

#### Start up a test server with the FR module installed

```bash
install commandbox-fusionreactor
server start
```

#### Navigate to the FR admin, login, and set your desired settings

```bash
fr open
```

#### Copy the populated `reactor.conf` file somewhere for later use

```bash
cp "`server info property=FRHomeDirectory`conf/reactor.conf" ./reactor.conf
```

Point your `server.json` to the new file.  Remember, non-absolute paths are relative to the directory the `server.json` lives in.

```bash
server set fusionreactor.reactorconfFile=reactor.conf
```

Now, a fresh new server will have these settings.

```bash
server stop 
server forget --force
server start
```

Note: the `reactor.conf` file may contain _passwords or other sensitive information_.  It is in a java properties file format, so feel free to edit it and remove items you don't want.  Also, take care when committing it to a source repo or making it web accessible so you don't reveal information.  There is currently no support for environment variable expansions in this file, but perhaps we'll add it if it's useful.

## Additional JVM args

The CommandBox FusionReactor module has passthrough settings for every documented JVM arg. Here are the remaining ones we haven't covered. If you want to know what some of these do, read [the official FR docs](https://docs.fusion-reactor.com/display/FR70/System+Configuration+Keys) on them.

Here's the module setting, followed by the JVM arg it creates. Remember, you can use environment variables in your `server.json` to control these dynamically on a per-server basis!

* **fusionreactor.password** - `fradminpassword`
* **fusionreactor.RESTRegisterURL** - `frregisterwith`
* **fusionreactor.RESTShutdownAction** - `frshutdownaction`
* **fusionreactor.RESTRegisterHostname** - `frregisterhostname`
* **fusionreactor.RESTRegisterGroup** - `frregistergroup`
* **fusionreactor.licenseDeactivateOnShutdown** - `frlicenseservice.deactivateOnShutdown`
* **fusionreactor.licenseLeaseTimeout** - `frlicenseservice.leasetime.hint`
* **fusionreactor.cloudGroup** - `fr.cloud.group`
* **fusionreactor.requestObfuscateParameters** - `fr.request.obfuscate.parameters`
* **fusionreactor.autoApplicationNaming** - `fr.application.auto_naming`
* **fusionreactor.defaultApplicationName** - `fr.application.name`

## External Server

By default, FusionReactor is only available on the FR port, and not the HTTP or HTTPS port.  If you want to hit FusionReactor's web UI through your main web server on the standard HTTP port, then enable the external server setting.

```bash
// Server specific version
server set fusionreactor.externalServerEnable=true
// global default version
config set server.defaults.fusionreactor.externalServerEnable=true
```

