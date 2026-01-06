---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/Bw6M3PI3e5HgLVcKZz0G/helpful-commands/token-replacements
---

# Token Replacements

One common thing in builds is replacing a token placeholder across all files, or perhaps certain files defined by a file globbing pattern. We've got a special command for that.

## From the CLI

```
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
