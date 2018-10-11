# Loading Ad hoc Jars

If you need to use a 3rd party jar, we recommend you use the extra parameters to the createObject\(\) function, which allows you to specify a list of jars to load from \(this is a Lucee-specific feature\).  Read up on the `context` parameter here:

{% embed url="http://docs.lucee.org/reference/functions/createobject.html" %}

There are some scenarios however that don't work.  One is if you need to use the `createDynamicProxy()` BIF to create CFC instances that implement Java classes that exist in an ad hoc jar.  Lucee currently requires those classes to be loaded by the Lucee system classloader.  

As such, CommandBox gives you a mechanism to load jars into the system class loader that was used to classload Lucee so those classes are available everywhere.  This is a much better and portable solution to dropping the jars in the `~/.CommandBox/lib` folder and restarting the shell.

## classLoad\(\)

To load up your custom jars on the fly, call the `classLoad()` method which is available in any custom command or Task Runner.

```javascript
classLoad( 'D:/amqp-client-5.1.2.jar' );
```

You can pass either an array or list of:

* D**irectories**   - All jar and class files will be loaded from each directory recursively
* **Jar files**   - Each jar file will be loaded
* **Class files** - Each class file will be loaded

Note, paths need to be absolute when you pass them in!  Here's some more examples.

```javascript
classLoad( 'C:/myLibs,C:/otherLibs' );
classLoad( [ 'C:/myLibs', 'C:/otherLibs' ] );
classLoad( 'C:/myLibs/myLib.jar,C:/otherLibs/other.class' );
classLoad( [ 'C:/myLibs/myLib.jar', 'C:/otherLibs/other.class' ] );
```

