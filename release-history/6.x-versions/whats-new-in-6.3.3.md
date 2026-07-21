# What's New in 6.3.3

This update changes the MAX_ENTITY_SIZE (the size of the request body) to `209715200` or 200 mb. When CommandBox 6.3.2 updated the Undertow version, it changed the default MAX_ENTITY_SIZE from unlimited to 2mb.

The value can be overridden in server.json with the following segment:

```
"runwar": {
    "UndertowOptions": {
        "MAX_ENTITY_SIZE":209715200
    }
}
```

## Release Notes ⚡️

Here are the full release notes for this release.

#### Bug

[COMMANDBOX-1686](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1686) File uploads larger than 2mb fail when using Commandbox 6.3.2

#### Improvements

[COMMANDBOX-1687](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1687) support rest mappings for BoxLang

