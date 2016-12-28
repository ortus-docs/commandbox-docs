# ColdFusion Settings

More and more people are starting to use CommandBox for their local development, especially among teams who want to quickly and easily start up the same environment on each of their machines.  That has led to the most common question now for CommandBox users which is:

> How do I automatically configure my ColdFusion/Lucee settings on my server?

This is a fair question, and for the most part we've treated configuring the settings in your CF engine to be outside the realm of what CommandBox tries to solve.  However the reality is, the CF engines often times fall short and people need a better way.  Here's a guide for your current options when it comes to configuring the settings on your CF engine.  These are listed in no particular order.

 







## Create a Custom Server Home
The previous section uses a one-time copy of settings, but doesn't give you a way to easily get changes made in the administrator back into your source control for a team to share.  For this we can use the `serverHomeDirectory` setting which overrides where the server WAR will be extracted to.  You can "seed" a directory with config files and then ask CommandBox to start your server in that directory, thus using your configs.  Then if you tell your source control to ignore all files but the configs, you can commit any changes to the configs back to source control for the rest of your team to get.  Here's how it works:

Consider the following `server.json`.  It defines a server who's web root will be in the `src` folder, but the actually server intallation will happen in the `serverHome` folder.
```
{
    "wwwroot" : "src",
    "app":{
        "serverHomeDirectory":"serverHome",
        "cfengine":"adobe"
    }
}
```

Now add this `.gitignore` in the `/serverHome` folder (assumes you're using Git for source control).  This will ignore all the files in the directory with the exception of the `.gitignore` file itself and all of your Adobe server's XML configs.

```
*
!/serverHome/WEB-INF/cfusion/lib/*.xml
!*/
!.gitignore
```

Running `start` will unzip the Adobe CF war into the `serverHome` folder.  All the new XML config files will show up to commit, which you can after making whatever setting changes you wish such as CF mappings, and datasources.  Note, the admin password is stored in the `password.properties` file so modify the ignored files if you wish to commit it.

Now, when your coworkers clone the repo, they will get a `serverHome` folder with nothing in it but the Adobe XML config files.  When they run `start` for the first time, the CF engine will be installed in the folder, using the pre-existing XML files which will automatically pick up all your config!  This is pretty sweet since it will let you deploy an app that comes pre-packaged with your engine's configs in a special folder prepared to be used as the server home.  

If you want to do the same thing for Lucee, modify the ignores as needed.  When a new version of an engine is available, CommandBox won't re-install the engine since it's already there and there will be a warning on the screen that you're trying to start a newer version than what's there and you need to `server forget` and start again to actually get the new engine.

## Conclusion
Hopefully this guide has given you some ideas on how to better package up your servers.  If you want to learn more about server.json in general, check out the [docs here](https://ortus.gitbooks.io/commandbox-documentation/content/embedded_server/serverJSON/serverjson.html).  Please use these examples a starting point and remember you can get even more funky by [creating a CommandBox module](https://ortus.gitbooks.io/commandbox-documentation/content/developing/modules/developing_modules.html) that listens to the `onServerInstall` or `onServerStart` interception points.  They will have access to much more data than the package scripts do.  

