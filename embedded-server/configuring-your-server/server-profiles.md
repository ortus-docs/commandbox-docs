# Server Profiles

CommandBox has profiles you can assign to a server when you start it to configure the default settings.  This is to provide easy secure-by-default setups for your production servers, and to make it easier to switch between a development mode and production mode.

There are 3 currently supported profiles.  Custom profiles will be added as a future feature.

* **Production** - Locked down for production hosting
* **Development** - Lax security for local development
* **None** - For backwards compat and custom setups. Doesn't apply any web server rules

## Setting the profile

You can set the profile for your server in your `server.json` 

```bash
server set profile=production
```

Which create this property

```javascript
{
  "profile": "production"
}
```

Or you can specify it when startign the server like so:

```bash
server start profile=production
```

### Default Profile

If a profile is not set, these rules are used to choose the default value:

* If there is an env var called `environment`, it is used to set the default profile \(same convention as ColdBox MVC\)
* If the site is bound on `localhost`, default the profile to "**development**".  Localhost is defined as any IP address starting with `127.`
* If neither of the above are true, the default profile is "**production**".  This makes CommandBox servers secure by default.

## **Production** profile

When profile is set to "**production**", the following defaults are provided:

* `web.directoryListing` = false
* `web.blockCFAdmin` = external
* `web.blockConfigPaths` = true

## Development profile

When profile is set to "**development**", the following defaults are provided:

* `web.directoryListing` = true
* `web.blockCFAdmin` = false
* `web.blockConfigPaths` = true

## None profile

When profile is set to "**none**", the following defaults are provided:

* `web.directoryListing` = true
* `web.blockCFAdmin` = false
* `web.blockConfigPaths` = false

## Customizing your profile

The defaults above only apply if you do not have am explicit `server.json` or `server.defaults` config setting.  If you have an explicit setting, it will override the profile's default.  Therefore, if you set the `profile` to`production` but set `web.blockCFAdmin` to `false`, your CF administrator will be public, but the remaining production defaults will still be applied.  This allows even the default profiles to be customizable.

```javascript
{
  "profile": "production"
  "web": {
    "blockCFAdmin": false
  }
}
```



