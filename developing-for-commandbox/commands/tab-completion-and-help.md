# Tab Completion & Help

Tab completion and help are powered by metadata on your command CFCs. For basic command and parameter tab completion to work you don't need to do anything. CommandBox will look at the metadata of your command and just use it.

## Command Help

When a user types `yourCommand help`, the text they see is the hint attribute on the component. The easiest way to add this is via a Javadoc-style comment at the top of your component. Feel free to include sample executions which can be wrapped in `{code:bash}{code}` blocks.

```javascript
/**
* This is a description of what this command does!
* This is how you call it:
*
* {code:bash}
* myCommand myParam
* {code} 
* 
**/
component {
    ...
}
```

## Parameter Help

You'll also want to provide hints for each of the parameters to the `run()` method. This will also be visible in the help command as well as the Command API docs.

```javascript
/**
* @param1.hint Description of the first parameter
* @param2.hint Description of the second parameter
*/
function run( required String param1, Boolean param2 ) {
    ...
}
```

## Parameter Value Completion

When a user hits tab while typing a parameter value, booleans will prompt with true/false. Parameters with `directory`, `destination`, `file`, or `path` in their names will automatically give file system suggestions.

You can provide additional hints though for your parameters to make them super user friendly.

If you have a parameter that doesn't contain one of the keywords above but you still want it to provide the user with path, directory, or file tab completion, you can add annotations to the arguments to enable this. &#x20;

* **optionsFileComplete**
* **optionsDirectoryComplete**

Ex:

```javascript
/**
* @myParam This is a parameter that will autocomplete files
* @myParam.optionsFileComplete true
* @yourParam This is a parameter that will autocomplete directories
* @yourParam.optionsDirectoryComplete true
*/
```

A parameter can have both the annotations for file and directory completion as well as an `optionsUDF` **** or `options` list specified and they will all be used.  The user will be presented with the combined list of tab completion candidates.

### Static Options

If a parameter has a set number of exact options, you can specify an `options` attribute for that parameter that is a comma-delimited list of options for tab completion to give.

```javascript
/**
* @gender.hint Please tell us your gender
* @gender.options Male,Female,Unknown
*/
function run( String gender ) {
    ...
}
```

### Options UDF

If a parameter's values are dynamic, you can specify a `optionsUDF` attribute for that parameters that points to a function name in the command CFC that will be called. This function needs to return an array of all possible values. CommandBox will do the work of filtering down partial matches if they're in the middle of typing.

```javascript
/**
* @forgeBoxType.hint What type of package is this?
* @forgeBoxType.optionsUDF completeTypes
*/
function run( String forgeBoxType ) {
    ...
}

array function completeTypes( string paramSoFar, struct passedNamedParameters ) {
    // Return array of possible types
    return [ 'type1', 'type2', 'type2' ];
}
```

An example of this in action is the install command. It will auto-complete the ForgeBox slug for you. Try typing `install cold` and hitting tab to see what happens.

Instead of just passing back an array of strings from your options UDF, you can pass an array of structs to provide the following for each option:

* name (required)
* group
* description

```javascript
array function completeTypes() {
 return [
  { name:'type1', group:'Types', description:'This is Type 1' },
  { name:'type2', group:'Types', description:'This is Type 2' },
  { name:'type3', group:'Types', description:'This is Type 3' }
 ];
}
```

## Smart Tab Complete

Your tab complete UDF is called every time the user hits tab, but sometimes the options you want to present are based on some context.  There are two parameters to your UDF which give you some context.

```javascript
array function completeTypes( string paramSoFar, struct passedNamedParameters ) {
   ...
}
```

* **paramSoFar** - A string containing the text the user has typed thus far.  CommandBox will automatically filter the candidates you send back, if if you're doing something complicated like making an API call to get the candidates, this allows you to do some pre-filtering of your own.  This string may be empty if they haven't typed anything yet.
* **passedNamedParameters** - A struct containing the parameters the user has typed already.  You get name/value pairs even if the user is typing positional params.  This is handy if the possible options for one parameter are based on what is being passed for another parameter.  The struct may be empty, or may only contain some params.  Always check existence!

## Namespace help

If you have a namespace (folder) of commands, you can control what the user sees when they type `namespace help` by creating a command CFC called `help.cfc` in the root of that folder. The help command should print out whatever information you want. CommandBox will automatically call it when the users needs help for that namespace.
