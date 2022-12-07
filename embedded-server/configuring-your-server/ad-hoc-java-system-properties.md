# Ad-Hoc Java System Properties

You've always been able to add ad-hoc Java system properties for a server in your `server.json` via `jvm.args` in the format of `-Dfoo=bar`.  There is also a top-level struct that is more readable which does the same thing:

```
{
  "jvm" : {
    "properties" : {
       "foo" : "bar baz"
       "java.awt.headless" : "true"
    }
}
```

No additional quoting or escaping is needed for spaces or special characters when using this method.

Set these programmatically like so:

```
server set jvm.properties.java.awt.headless=true
```

Or set them globally for all servers in your config setting server defaults.

```
config set server.defaults.jvm.properties.java.awt.headless=true
```

Keys will be merged, giving precedence to the `server.json` values.
