# REST Servlet

The built in REST implementation in Adobe ColdFusion and Lucee is usually something you either love or hate. If you love it, you can enable it and customize the paths so it doesn't collide with your app's routes. if you hate it, you can turn it off. The REST servlet will be disabled unless you include a setting like so:

```text
server set app.restMappings=/rest/*,/api/*
```

Note that the above setting will take over any URLs starting with `/rest` or `api` and you cannot use those routes or folders in your code. This is why it's handy to be able to modify or remove these. On a typical server, this is done via the `web.xml`, but CommandBox will do it all for you with just the setting above.

