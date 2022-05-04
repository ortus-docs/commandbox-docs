# What's in 1.0.0

![](https://www.ortussolutions.com/\_\_media/commandbox-185-logo.png)

After almost a year in development, we are so excited to finally announce the release of [CommandBox 1.0.0 Final](https://www.ortussolutions.com/products/commandbox).  This has been definitely one of the most challenging and fun projects we have overtaken here at Ortus.  We had a vision of how we could accelerate not only development, tools and ultimately the ColdFusion (CFML) landscape by building a tool that could put us up to par with many other technologies.  I am glad to say we have now a great foundation to move forward.  CommandBox brings CFML to any Operating System and even embedded systems like the Raspberry and Banana Pi.  It also gives ColdFusion (CFML) developers a much better workflow to work with their projects and a sense of community we lovingly call [ForgeBox](http://www.coldbox.org/forgebox). &#x20;

With anything we do here at Ortus, it is fully documented using our new book formats.  So head on over to [commandbox.ortusbooks.com](http://commandbox.ortusbooks.com) to download or read the entire CommandBox documentation.  In the next coming weeks we will begin our CommandBox 5-week roadshow that will include weekly blogging tutorials and video presentations, so stay tuned as each week progresses.  So without further ado, I present to you project Gideon: CommandBox CLI!

## CommandBox

CommandBox is a standalone, native tool for Windows, Mac, and Linux.  It provides a Command Line Interface (CLI) for developer productivity, tool interaction, package management, embedded CFML server, application scaffolding, and some sweet ASCII art.  It includes a plethora of commands to interact with your Operating System, TestBox, ForgeBox, ContentBox, CacheBox, etc.  Built-in help is completely integrated for every command.  You can pop open a CommandBox shell in your terminal window and manually type commands, or even automate things externally via the CommandBox binary with your OS's native shell. &#x20;

* [Download & Install CommandBox](https://www.ortussolutions.com/products/commandbox)
* [Download-Read CommandBox Manual](http://commandbox.ortusbooks.com)
* [ForgeBox](http://www.coldbox.org/forgeBox)
* [Vimeo Video Channel](https://vimeo.com/channels/commandbox)

## Package Management

So one of the biggest things we think the CFML community was missing, was a true package management platform.  With this in mind, CommandBox + ForgeBox now includes full package management control for ANY ColdFusion (CFML) application.  We have created a spec for a box.json file which will go in the root of CFML packages to describe metadata about the package, how it should be installed, and dependencies that the package requires to run.  CommandBox is getting a tight integration with the ForgeBox REST API to search, view, and install packages/modules directly into your app from the command line.

## REPL: Read-Evaluate-Print-Loop

The CommandBox CLI also leverages a REPL console for executing a-la-carte CFML commands. You can use it in script or even tag mode with full command history as well. Each REPL instance also has included memory, which means you can declare functions, datasources, etc and leverage them within the same command executions. We even support multi-line statements.

## Application Scaffolding

CommandBox has tons of commands for quickly building out applications.  Create a new ColdBox app with `coldbox create app`, add a handler with `coldbox create handler`.  You can even get actions added to it, views created, and BDD integration tests stubbed out at the same time.  This can bring new productivity for people who like to live on the command line and especially for those who want to be able to automate stuff they do a lot of.

## Extensible

CommandBox has a thin Java layer and a rich CFML command suite built using WireBox dependency injection. This allows for any CFML developer to contribute and write out their own commands. You can even register your [commands in ForgeBox](http://www.coldbox.org/forgebox/type/commandbox-commands) and have them available to any CommandBox installation. This means that any application or framework author can contribute their own suite of commands for their community.

## Automation

CommandBox also leverages the concept of CommandBox Recipes which allows you to create reusable command files with a `box` extension. You can execute this recipes and even do argument-binding for further reusability. You can even [shared them in ForgeBox](http://www.coldbox.org/forgebox/type/commandbox-recipes) as well.  It also natively integrates into your operating system you can even use CommandBox for Unix shell scripting or just plain template executions: `box myfile.cfm` or even use argument-binding `box execute file=mayflies.cfm var1=hello name=luis` and we will bind those variables into the `variables` scope for you.

## Embedded Server

One of the cool things CommandBox brings to the table is the ability to spin up an ad hoc, lightweight, CFML server in any directory from the command line.  Simply change your working directory to the root of your app, type `server start` and a super fast CFML server spins up on a new port running your code.  When you're done type `server stop` from that directory or use the little icon that's showed up in your system tray.In our final release we even included SSL and full URL rewrite support as well.

## Auto Updates

We have spent considerable time in our auto-update capabilities so users can transition to patches and updates with ease. We have even created two channels for updates:

* **Stable** : Stable releases
* **Bleeding Edge** : Bleeding edge releases

So from you console you can type: `box upgrade --latest` for bleeding edge releases or `box upgrade` for stable releases.

## Future RoadMap

In the next coming months, ForgeBox 2.0 will be released with many more features to help developers manage their contributions, multiple version control, CommandBox integration, private repositories and much more.  We will also be using the URL **forgebox.io** instead of embedding it in the ColdBox site; time for separation.  We also have tons of features planned for CommandBox, here are a few teasers:

* Adobe CF embedded server
* Task Runner
* NodeJS bridges
* Lucee Support
* Multiple installation providers
* ForgeBox Enterprise (For private enterprise installations)
* ForgeBox Cloud Private Entries
* RCE (Let's see if you can figure out the acronym)
* Multiple version and fuzzy version package management
* WAR packager
* Package signing
* Much More

So as you can see, so much work to be done.  I leave you with one final note, we highly encourage you to [support us](https://www.ortussolutions.com/services) in any way you can as ultimately we offer these tools as professional open source and they need your support in order to continue with their development.  Enjoy and go code something!
