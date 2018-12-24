# Using File Globs

If you want users to be able to pass file globbing patterns to your command, set a type of `Globber` for the argument.

```javascript
/**
* @path.hint file or directory to interact with.
**/
function run( required Globber path )  { }
```

Even though the user types a string, CommandBox will hand you a CFC instance that's preloaded with the matching paths and has some nice methods to help you interact with the matching paths. The Globber object lazy loads the matches, which gives you time to affect the pattern or even the sort if you wish prior to the file system actually being hit.

## Globber.count\(\)

Use the `count()` method to get the total number of paths matched on the file system.

```javascript
function run( required Globber path ) {
  print.line( path.count() & ' files affected!' );
}
```

## Globber.apply\( ... \)

The easiest way to apply some processing to each of the file paths found is by passing a closure to the `apply()` method. The closure will be executed once for each result.

```javascript
function run( required Globber path ) {
  path.apply( function( thisPath ){
    print.line( 'Processing file ' & thisPath );
  } );
}
```

## Globber.matches\(\)

To get the raw results back, use the `matches()` method.

### Globber.asQuery\(\)

If you want to get the results back as a query object, use the `asQuery()` method and then you can loop over them yourself. The query contents match what comes back from `directoryList()`.

```javascript
function run( required Globber path ) {
var myQry = path
  .asQuery()
  .matches();
}
```

### Globber.asArray\(\)

If you want to get the results back as an array, use the `asArray()` method and then you can loop over them yourself.

```javascript
function run( required Globber path ) {
var myArr = path
  .asArray()
  .matches();
}
```

## Globber.withSort\( ... \)

Affect the order that the results come back by setting a sort. The sort follows the same pattern as the directoryList\(\) function, which means it can be a comma-delimited list of columns to sort on.

```javascript
function run( required Globber path ) {
var myQry = path
  .asQuery()
  .withSort( 'type, name' )
  .matches();
}
```

## Globber.getPattern\(\)

To get the original globbing pattern that the user typed, use the `getPattern()` method on the Globber CFC.

```javascript
function run( required Globber path ) {
  print.line( 'You typed ' & path.getPattern() );
}
```



