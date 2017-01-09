# Application.cfc
This is my favorite and recommended approach.  It's automatic, keeps your configuration with your codebase, and requires no manual process.  The biggest issue with configuring your app server's settings in `Application.cfc` is that Adobe ColdFusion doesn't provide nearly the same amount of configuration as Lucee, and older versions of Adobe CF don't allow any at all.  Lucee isn't 100% either.  For instance, mail servers and cache connections just got added there in recent releases.  Here's a quick run down:

* **Adobe CF 9** - CF mappings only
* **Adobe CF 10** - CF mappings only
* **Adobe CF 11** - Adds support for data sources ([link](http://blogs.coldfusion.com/post.cfm/application-datasources-in-coldfusion))
* **Adobe CF 2016** - A few small [additions](http://cfdocs.org/application-cfc)
* **Lucee 4.x** - Many non-compilation server settings (See setting export page in admin)
* **Lucee 5.x** - Most non-compilation server settings (See setting export page in admin)

So unless you're on Lucee 5, you're going to be limited.  I'm not aware of a comprehensive overview of Adobe settings available other than [this](http://cfdocs.org/application-cfc), but here is a [Google doc](https://docs.google.com/spreadsheets/d/10s-nn_FsoSD_RiLwjYZICacCoC386SjkEGT3pOfBJVU/edit?usp=sharing) that shows an overview of all the possible Lucee 5 configurations.  Check the `Application.cfc` column.  

In the Lucee admin, you can click the little question mark under any setting and it will show you what to copy paste into your `Application.cfc` to set the equivalent setting in your code. There's also an option to export all your Lucee config at once.

For an example of an Application-specific datasource in Adobe CF11+, [click here](https://github.com/foundeo/cfml-security-training/blob/master/wwwroot/Application.cfc#L15-L22).

