# ColdFusion Settings

More and more people are starting to use CommandBox for their local development, especially among teams who want to quickly and easily start up the same environment on each of their machines.  That has led to the most common question now for CommandBox users which is:

> How do I automatically configure my ColdFusion/Lucee settings on my server?

This is a fair question, and for the most part we've treated configuring the settings in your CF engine to be outside the realm of what CommandBox tries to solve.  However the reality is, the CF engines often times fall short and people need a better way.  Here's a guide for your current options when it comes to configuring the settings on your CF engine.  These are listed in no particular order.

 









## Conclusion
Hopefully this guide has given you some ideas on how to better package up your servers.  If you want to learn more about server.json in general, check out the [docs here](https://ortus.gitbooks.io/commandbox-documentation/content/embedded_server/serverJSON/serverjson.html).  Please use these examples a starting point and remember you can get even more funky by [creating a CommandBox module](https://ortus.gitbooks.io/commandbox-documentation/content/developing/modules/developing_modules.html) that listens to the `onServerInstall` or `onServerStart` interception points.  They will have access to much more data than the package scripts do.  

