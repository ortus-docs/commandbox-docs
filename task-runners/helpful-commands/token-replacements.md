# Token Replacements

One common thing in builds is replacing a token placeholder across all files, or perhaps certain files defined by a file globbing pattern. We've got a special command for that.

## From the CLI

```text
tokenReplace path=/tests/*.cfc token="@@version@@" replacement=`package version`
```

## From CFML

```javascript
command( 'tokenReplace' )
    .params( 
        path = "/tests/*.cfc",
        token = "@@version@@",
        replacement = command( 'package version' ).run()
     )
    .run();
```

