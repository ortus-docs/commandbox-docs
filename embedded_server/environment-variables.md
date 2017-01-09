# Environment variables/Java properties
This is a Lucee 5-only option, not supported on Adobe CF or Lucee 4.x servers at all.  A select number of Lucee settings can be specified as environment variables or Java system properties prior to starting the server.  These are ideal for compilation settings which can't be specified in code.  Here is a [Google doc](https://docs.google.com/spreadsheets/d/10s-nn_FsoSD_RiLwjYZICacCoC386SjkEGT3pOfBJVU/edit?usp=sharing) that shows an overview of all the possible Lucee configurations.  Check the `EnvVar/SysProp` column.  Here are a few big ones:

* **lucee.full.null.support** (boolean)
* **lucee.web.dir** (path)
* **lucee.base.dir** (path)
* **lucee.enable.dialect** (boolean)

To use these on a CommandBox server, you can set them as environment variables in your operating system, or package them as Java system properties in your server's `server.json` like so:

```js
{
  "jvm":{
    "args":"-Dlucee.enable.dialect=true"
  }
}
```

Basically, prefix the property name with "`-D`".  This is the standard Java way to set system properties via JVM args.  When the server starts, those Java system properties will be defined and picked up by the engine.

