# Experimental Features

{% hint style="danger" %}
Note, as of CommandBox 6, the old format of experimental Runwar args that look like `--name-here` is removed.  There is a new syntax for specifying arbitrary experimental flags.
{% endhint %}

There are not any experimental features in CommandBox, but you can influence the JSON file used to configure runwar with the following syntax.

```bash
server set runwar.options.path.to.option="value"
```

```javascript
{
  "runwar" : {
    "options" : {
      "path" : {
        "to" : {
          "option" : "value"
        }
      }
    }
  }
}
```

The struct you specify will be merged into the JSON file that is sent to Runwar. Note, this JSON format is undocumented and subject to change. &#x20;
