# Passing Parameters

A task target can defined as many method arguments as it wants which can be passed in via command arguments when calling the task.

**fun.cfc**

```javascript
component{
    function greet( string name, boolean verbose ){
        print.line( 'Well, hello there #name#!' );
        if( verbose ) {
            print.line( "You're looking quite well today." );
        }
    }
}
```

There's two ways to pass in the `name` and `verbose` parameters: positionally and via named parameters.

## Positional

If you want to pass your parameters positionally, you **must include the task and target name**.

```text
task run fun greet Brad false
```

## Named

A more self-documenting method is to use named parameters. Note, it is not necessary to pass the task and target name when using named parameters, but in this case, my example does not use the default task and target convention names, so I'll need to pass them anyway. Note that we start each parameter name with a colon \(`:`\) so they don't interfere with any of the parameters to the actual `task run` command.

```text
task run taskFile=fun target=greet :name=Brad :verbose=false
```

The parameters `:name` and `:verbose` will be passed directly along to the task as `name` and `verbose`.

## Flags

Tasks with boolean parameters can also have those passed using flags just like commands. Simply prepend a colon \(`:`\) to the name of the flag like so.

```text
task run --no:verbose
or
task run --!:verbose
```

## Mix it up

Since `task run` is just a regular command, remember its parameters don't have to be hard coded. They can expressions or system settings, etc.

```text
task run ${APIDocURL} ${APIDocPort:8080}
task run taskFile=build :message=`cat message.txt`
```

