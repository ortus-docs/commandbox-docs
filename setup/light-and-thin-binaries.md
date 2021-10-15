# Light and Thin Binaries

If you are just installing CommandBox to use on your PC as a local development tool, the standard version  of the box binary should be fine. It contains a full version of Lucee with all its default extensions pre-installed.  However, if you are creating an automation or a distributable such as a Docker container you may want to look into one of these alternative box binaries.

## CommandBox Light

CommandBox Light does not include the full version of Lucee, but rather Lucee Light which only comes with the Compress extension (because CommandBox requires this extension to run).  The CommandBox Light binary is roughly half the size of the normal version.  The on-disk size of the CommandBox home is about 1/3rd of the size.  This can be handy for places where disk size is very important.

Starting a default web server in CommandBox will give you a Lucee Light server, which may not run your app since it lacks extensions such as JBDC drivers and the admin.  You can still ask for a specific version of Lucee with the `cfengine` parameter.  If you delete the `engine` folder from a CommandBox Light installation, Lucee will not be able to work since the Lucee.jar file in CommandBox Light has had it's `core.lco` file removed to make it smaller.  Deleting the entire `.CommandBox` folder will work however.

Any Task Runners or CLI automations in CommandBox light will also not be able to use things like JDBC drivers unless you install the extensions. Extensions can be installed by placing them in the CLI's server context deploy folder and waiting a minute:

```
~/.CommandBox/engine/cfml/cli/lucee-server/deploy
```

You can download the CommandBox Light binary directly from our S3 artifacts repo:

{% embed url="https://downloads.ortussolutions.com/#/ortussolutions/commandbox/" %}

* **box-light** - Mac/Linux
* **box-light.exe** - Windows
* **box-light.jar** - Executable jar

## CommandBox Thin

The CommandBox Thin binary cannot be used by itself as it contains nothing inside of it but the Java bootstrap.  It does not contain any of the other libraries, jars, or Lucee versions that CommandBox needs to load.  it is only 300KB in size.  The way that the 'thin" binary is to be used is to first download and run the normal CommandBox or CommandBox Light binary which will extract itself into the CommandBox Home.  Once this has been completed, you can delete that box binary and replace it with the box-thin binary (renamed to `box` ).  The new thin binary will take up less disk space and will simply use the existing CommandBox home that has been extracted. 

The thin binary does not care whether the CommandBox home was extracted with a full or "light" version.  This can create additional disk savings in an environment like Docker, but would not serve much of a purpose on your local development PC.    If you delete your `.CommandBox `folder, the CommandBox thin binary will not be able to recreate it.

You can download the CommandBox Light binary directly from our S3 artifacts repo:

{% embed url="https://downloads.ortussolutions.com/#/ortussolutions/commandbox/" %}

* **box-thin** - Mac/Linux
* **box-thin.exe** - Windows
* **box-thin.jar** - Executable jar
