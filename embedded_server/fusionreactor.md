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
If you don't have a license, you can sign up for a trial and purchase a license from the http://www.fusion-reactor.com/ website.


