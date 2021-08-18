# Debugging Server Rules

And to test/debug your rules, start your server with the trace flag and tail the console output. Every hit in your browser will output several lines of debugging for each rule that is processed.

```bash
server start --trace --console 
```

With the custom rule above, you'd see something like this:

```text
[DEBUG] requested: '/CFIDE/main/ide.cfm'
[TRACE] io.undertow.predicate: Path(s) [/CFIDE/main/ide.cfm] MATCH input [/CFIDE/main/ide.cfm] for HttpServerExchange{ GET /CFIDE/main/ide.cfm}.
[TRACE] io.undertow.predicate: Predicate [path( '/CFIDE/main/ide.cfm' )] resolved to true. Next handler is [done] for HttpServerExchange{ GET /CFIDE/main/ide.cfm}.
[TRACE] io.undertow.predicate: Predicate chain marked done. Next handler is [Runwar PathHandler] for HttpServerExchange{ GET /CFIDE/main/ide.cfm}.
[WARN ] responded: Status Code 200 (/CFIDE/main/ide.cfm)
```

