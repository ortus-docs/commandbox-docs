# CFML from the command line

CommandBox's true power comes from it's command-based architecture, but we also support just running plain-jane .cfm files as well. 

## Running plain CFML files

Take the following file for example:

**test.cfm**

```HTML
<cfoutput>#now()#</cfoutput>
```

We can execute this file directly from our native OS prompt by simply passing the filename straight into the box binary.

```bash
C:\> box test.cfm
{ts '2015-02-19 20:14:13'}
```

Or, I can run it from within the CommandBox interactive shell using the execute command:

```bash
CommandBox> execute test.cfm
{ts '2015-02-19 20:12:41'}
```
 
## \#! Goodness
Now, you people on Unix-based operating systems like Mac and Linux get a special treat.  You can actually create natively executable shell scripts that contain CFML!  Check out this file that has the special hash bang at top:

**test**

```bash
#!/usr/bin/env box

<cfoutput>#now()#</cfoutput>
```

All we need to do is make it executable
```bash
chmod +x test
```
And then just run it like any other shell script!
```bash
$> ./test

{ts '2015-02-19 20:31:32'}
```

Hopefully this gives you a lot of ideas of how to start using CFML on your next automation task.  And if you want even more control like print objects, object oriented code, and fancy parameters, look into making custom [CommandBox commands](http://commandbox.ortusbooks.com/content/developing/commands/developing_commands.html).


