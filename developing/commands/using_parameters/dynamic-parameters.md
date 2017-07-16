# Dynamic Command Parameters

Sometimes you have a command that can need an undetermined number of configuration items that all relate to the same thing.  CommandBox can accept any number of named or positional parameters, but it can be hard to collect a dynamic and unknown number of parameters.  An easy way to handle this is to have the user prefix each grouped parameter name with `something:` like this.

```
doWidget widgetProp:foo=blue widgetProp:bar=seven widgetProp:baz=turkey
```
This will create a single item in the `arguments` scope of your command called `widgetProp` which is a struct and contains the keys `foo`, `bar`, and `baz`.  You can loop over the `widgetProp` struct to access the collection in a concise manner.  You can have as any of these parameter groups as you want, and a struct will be created for each unique parameter prefix.

You can omit the prefix entirely and just start a collection of parameters with a colon (`:`) and they will be placed in a struct called `args`.

```
doWidget :foo=blue :bar=seven :baz=turkey
```

And the command might look like this:

```js
function run( args={} ){
    args.each( function( k, v ) {
        print.line( '#k# has a value of #v#.' );
    } );
}
```
Which, given the parameters shown above, would output this:
```
foo has a value of blue.
bar has a value of seven.
baz has a value of turkey.
```