# Property Files

If you're touching Java, there's probably some property files in your future. We've included the `PropertyFile` module in CommandBox that you can call directly from CFML. There are also some commands so you can script the creation and updating of property files from the command line and CommandBox recipes.

## From the CLI

```text
propertyFile show foo.properties
propertyFile set propertyFilePath=foo.properties newProp=newValue
propertyFile clear foo.properties newProp
```

## From CFML

```javascript
// Create and load property file object
propertyFile( 'myFile.properties' )
    .set( 'my.new.property', 'my value' )
    .store();

// Get a property
var value = propertyFile( 'myFile.properties' )
    .get( 'existing.property' );

// Create one from scratch
propertyFile()
    .set( 'brad', 'wood' )
    .store( 'myFile.properties' );
```

A `propertyFile` CFC instance can also be treated as a struct as it stores the properties in its `this` scope.

```javascript
// Create object
var propFile = propertyFile( 'myFile.properties' );

// Access proeprties
print.line( propFile.brad );

// Change/add properties
propFile.foobar = true;
propFile[ 'another.new.property' ] = false;

// Save it
propFile.store();
```

