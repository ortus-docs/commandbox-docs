# Aliases

CommandBox allows you to create web aliases for the web server that are similar to virtual directories. The alias path is relative to the folder the `server.json` lives in, but can point to any folder on the hard drive. Aliases can be used for static or CFM files.

{% hint style="info" %}
For compatibility reasons as of version `5.3.0`, relative alias paths can also be relative to the web root, if that is not where the `server.json` lives.  CommandBox will check if the folder exists relative to the web root FIRST, and if not, it will then check relative to the folder the `server.json` lives in.
{% endhint %}

To configure aliases for your server, create an object under `web` called `aliases`. The keys are the web-accessible virtual paths and the corresponding values are the relative or absolute path to the folder the alias points to.

Here's what your `server.json` might look like.

```javascript
{
  "web" : {
    "aliases" : {
      "/foo" : "bar",
      "/js" : "C:\static\shared\javascript"
    }
  }
}
```

Here's how to create aliases from the `server set` command:

```
server set web.aliases./foo = bar
server set web.aliases./js = C:\static\shared\javascript
```

> **info** On Adobe ColdFusion servers, .cfm files will be run automatically from inside an aliases directory. On Railo and Lucee servers, you'll need to create a CF mapping that maps the alias name and path for .cfm files to work.

{% hint style="info" %}
For [Multi-Site](../multi-site-support/), web alias settings can be configured on a per-site basis in the `sites` object of the `server.json` or in a `.site.json` file.
{% endhint %}

