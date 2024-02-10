# MIME Types

CommandBox will automatically set the content type in the HTTP response for common static file types. If you come across a file extention that doesn't have the correct type, you can set it like so in your `server.json`:

```bash
server set web.mimeTypes.log=text/plain
```

Which creates the following

```json
{                            
    "web":{                             
        "mimeTypes":{                  
            "log":"text/plain"      
        }
    }
} 
```

In the above example, hitting a file such as `foo.log` would come back with a `text/plain` content type header.

This setting will override any `<mime-mapping>` tag in your `web.xml` file.

{% hint style="info" %}
For [Multi-Site](../multi-site-support/), MIME Type settings can be configured on a per-site basis in the `sites` object of the `server.json` or in a `.site.json` file.
{% endhint %}

