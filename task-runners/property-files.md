# Property files
If you're touching Java, there's probably some property files in your future.  We've included the `PropertyFile` module in CommandBox that you can call directly from CFML.  There are also some commands so you can script the creation and updating of property files from the command line and CommandBox recipes.

## From the CLI

```
propertyFile show foo.properties
propertyFile set propertyFilePath=foo.properties newProp=newValue
propertyFile clear foo.properties newProp
```

## From CFML

```js
// Create and load property file object
propertyFile( 'myFile.properties' )
	.set( 'my.new.property', 'my value' )
	.store();
```
