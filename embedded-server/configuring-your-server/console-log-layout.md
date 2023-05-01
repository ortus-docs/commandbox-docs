# Console Log Layout

CommandBox servers use Log4j to process the console logs for a server.  The console logs contain&#x20;

* Debugging output from CommandBox
* Logging from any Java libraries in use such as Lucee or Undertow
* Output from Lucee's `systemOutput()` BIF
* Output from `<cfdump var="foo" output="console">`
* Output from LogBox's Console Appender

This output is visible in the "standard out" for the actual Runwar java process, which is what you see in Docker container logs, or if you start your server with the `--console` flag.  The output is also written file a file appender to the `server.out.txt` log file inside the CommandBox server home, which is what the `server log` command uses.&#x20;

By default Log4j's console and file appender use a `PatternLayout` with a default pattern of

```
[%-5p] %c: %m%n
```

The Log4j docs explain what valid placeholders exist

{% embed url="https://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/PatternLayout.html" %}

### Customize the PatternLayout's pattern

You can choose a custom pattern for the pattern layout.  This example would put the date/time into every log message:

```bash
server set runwar.console.appenderLayoutOptions.pattern="[%-5p] %d{dd MMM yyyy HH:mm:ss.SSS} %c: %m%n"
```

This example would log ONLY the message with no severity or category:

```bash
server set runwar.console.appenderLayoutOptions.pattern="%m%n"
```

Note, the color coding of log lines in CommandBox is dependent upon the default Log4j pattern layout.

### Customize Layout

If you want, you can change the entire appender layout itself to be something other than the `PatternLayout`.  Log4j supports a number of appender layouts plus is configurable with layouts of your own creation, so long as they are visible on the classpath.  All of the Log4j 2.x appender layouts are avilable for you to use as well as the `JSONTemplateLayout` which we also bundle. &#x20;

You can view all the built in layouts here:

{% embed url="https://logging.apache.org/log4j/2.x/manual/layouts.html" %}

Common options are:

* `HTMLLayout`
* `JSONTemplateLayout`
* `XMLLayout`
* `CsvLogEventLayout`
* `Rfc5424Layout`

Specify the layout like so:

```bash
server set runwar.console.appenderLayout=JSONTemplateLayout
```

Any settings specific to a layout can be set just like the pattern option above. &#x20;

```bash
server set runwar.console.appenderLayoutOptions.stackTraceEnabled=false
server set runwar.console.appenderLayoutOptions.maxStringLength=250
```

```json
{
    "runwar":{
        "console":{
            "appenderLayout":"CsvLogEventLayout"
        }
    },
     "app":{
        "libDirs":"libs/commons-csv-1.10.0.jar"
    }
}
```

Please refer to the Log4j docs to find what the valid options are for a given appender layout.

Note: some of the built in layouts require additional jars which do not ship with CommandBox. You will, need to download these jars separately and specify them to the `app.libDirs` setting so they are visible to the class loader.  For example, the CSV layout requires the Apache Commons `commons-csv` library which you would specify like so:

```json
{
    "runwar":{
        "console":{
            "appenderLayout":"JSONTemplateLayout",
            "appenderLayoutOptions":{
                "stackTraceEnabled":false,
                "maxStringLength":250
            }
        }
    }
}
```
