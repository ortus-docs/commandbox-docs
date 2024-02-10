# Server Rules

CommandBox servers have a method of locking down secure URLs and or implementing any of the Undertow predicate and handlers via a nice text based language. Undertow supports a “predicate language” that allows a string to be parsed into a graph of predicates (conditions) and handlers (actions to take). Ex:

```javascript
path-suffix-nocase(/box.json) -> set-error(404)
```

These rules can be used for any of the following:

* **Security** - Block paths, IPs, or users
* **URL rewrites** - Rewrite incoming URLs to something different
* **Modifying HTTP requests on the fly** - Set headers, cookies, or response codes

Much of this functionality overlaps with the existing Tuckey-based rewrites in CommandBox, but this functionality is built directly into Undertow, has a more streamlined syntax, and allows for easier ad-hoc rules to be layered into a server that allows for you to have custom rules layered on top of built in rules. It can be used to replace what Tuckey does, or added on top.

{% hint style="info" %}
If you have Tuckey rewrites enabled AND use the Undertow predicate-based server rules, the server rules will fire BEFORE the Tuckey rewrites.
{% endhint %}

## Create your Rules

Unlike custom Tuckey-based rewrites that must be placed in a single XML file, server rules can be provided ad-hoc in a variety of locations. They are combined and passed to the server in the order defined. This allows you to easily "layer" custom rules along with out-of-the-box lockdown profiles.

For maximum configuration options, the following mechanisms are supported for specifying the rules for a given server. Rules are processed in the order listed. i.e., a rule defined in your `server.json` is processed prior to a rule in your `server.default` config setting.

1. Ad-hoc rule array in  `sites` object or `.site.json` for Multi-site server
2. Ad-hoc rule array in `server.json`
3. External rules files in the `sites` object of the `server.json` or `.site.json` for Multi-site server
4. External rules files in `server.json` in the order defined
5. Ad-hoc rule array in config setting `server.defaults`
6. External rules files in config setting `server.defaults` in the order defined
7. CommandBox built-in rules (`web.blockCFAdmin`, `web.blockSensitivePaths`)
8. Any module listening to server interceptions can inject their rules _wherever they please in the array_.

### `server.json` Rules

You can specify ad-hoc rules in the `web.rules` property as an array of strings in your `server.json` as well as specify one or more external rule files in the `web.rulesFile` property as an array or list of file glob patterns.

```javascript
{
    "web" : {
        "rules" : [
            "path-suffix-nocase(/box.json) -> set-error(404)",
            "path-suffix-nocase(hidden.js) -> set-error(404)",
            "path-prefix-nocase(/admin/) -> ip-access-control(192.168.0.* allow)",
            "path(/sitemap.xml) -> rewrite(/sitemap.cfm)",
        "disallowed-methods(trace)"
        ],
          "rulesFile" : "../secure-rules.json"
        // Or...
          "rulesFile" : ["../security.json","../rewrites.txt","../app-headers.json"]
        // Or...
          "rulesFile" : "../rules/*.json"
    }
}
```

External rule files with a `.json` suffix will be expected to be a valid JSON file containing an array of strings. Ex:

{% code title="myRuleFile.json" %}
```javascript
[
  "path-suffix-nocase(/box.json) -> set-error(404)",
  "path-suffix-nocase(hidden.js) -> set-error(404)"
]
```
{% endcode %}

External rule files with any extension OTHER than `.json` will be expected to be a raw text file with one rule per line. Emtpy lines are ignored and the rules are processed in the order defined.

{% code title="myRuleFile.txt" %}
```
path-suffix-nocase(/box.json) -> set-error(404)
path-suffix-nocase(hidden.js) -> set-error(404)
```
{% endcode %}

Rules specified directly in the `server.json` or in an external JSON file must be escaped for the JSON they are a part of. Using a plain text external file can help readability since no additional escaping is required for the rules.

### config setting `server.defaults` Rules

Like all other config server defaults, they follow the same pattern as the `server.json` file.

```bash
config set server.defaults.web.ruleFile=/path/to/rules.json
```

{% hint style="info" %}
For [Multi-Site](../../multi-site-support/), server rule settings can be configured on a per-site basis in the `sites` object of the `server.json` or in a `.site.json` file.
{% endhint %}



## Commenting Out Rules

You can comment out any rule (whether it's in JSON or a text file) by proceeding it with a pound sign (`#`).

{% code title="myRuleFile.txt" %}
```
# Here is a comment that will be ignored

# The following rule also won't be run
# path-suffix-nocase(hidden.js) -> set-error(404)
```
{% endcode %}
