# Server Rules

CommandBox servers have a method of locking down secure URLs and or implementing any of the Undertow predicate and handlers via a nice text based language.  Undertow supports a “predicate language” that allows a string to be parsed into a graph of predicates \(conditions\) and handlers \(actions to take\).  Ex:

```javascript
path-suffix(/box.json) -> set-error(404)
```

These rules can be used for any of the following:

* **Security** - Block paths, IPs, or users
* **URL rewrites** - Rewrite incoming URLs to something different
* **Modifying HTTP requests on the fly** - Set headers, cookies, or response codes

Much of this functionality overlaps with the existing Tuckey-based rewrites in CommandBox, but this functionality is built directly into Undertow, has a more streamlined syntax, and allows for easier ad-hoc rules to be layered into a server that allows for you to have custom rules layered on top of built in rules.  It can be used to replace what Tuckey does, or added on top.  

{% hint style="info" %}
If you have Tuckey rewrites enabled AND use the Undertow predicate-based server rules, the server rules will fire BEFORE the Tuckey rewrites.  
{% endhint %}

## Create your Rules

Unlike custom Tuckey-based rewrites that must be placed in a single XML file, sever rule can be provided ad-hoc in a variety of locations.  They are combined and passed to the server in the order defined.  This allows you to easily "layer" custom rules along with out-of-the-box lockdown profiles.

For maximum configuration options, the following mechanisms are supported for specifying the rules for a given server.  Rules are processed in the order listed.  i.e., a rule defined in your `server.json` is processed prior to a rule in your `server.default` config setting.

1. Ad-hoc rule array in `server.json`
2. External rules files in `server.json` in the order defined
3. Ad-hoc rule array in config setting `server.defaults`
4. External rules files in config setting `server.defaults` in the order defined
5. CommandBox built-in rules \(`web.blockCFAdmin`, `web.blockConfigPaths`\)
6. Any module listening to server interceptions can inject their rules _wherever they please in the array_.

### `server.json` Rules

You can specify ad-hoc rules in the `web.rules` property as an array of strings in your `server.json` as well as specify one or more external rule files in the `web.rulesFile` property as an array or list of file glob patterns.  

```javascript
{
    "web" : {
        "rules" : [
            "path-suffix(/box.json) -> set-error(404)",
            "path-suffix(hidden.js) -> set-error(404)",
            "path-prefix(/admin/) -> ip-access-control(192.168.0.* allow)",
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
  "path-suffix(/box.json) -> set-error(404)",
  "path-suffix(hidden.js) -> set-error(404)"
]
```
{% endcode %}

External rule files with any extension OTHER than `.json` will be expected to be a raw text file with one rule per line.  Emtpy lines are ignored and the rules are processed in the order defined.

{% code title="myRuleFile.txt" %}
```text
path-suffix(/box.json) -> set-error(404)
path-suffix(hidden.js) -> set-error(404)
```
{% endcode %}

Rules specified directly in the `server.json` or in an external JSON file must be escaped for the JSON they are a part of.  Using a plain text external file can help readability since no additional escaping is required for the rules.

### config setting `server.defaults` Rules

Like all other config server defaults, they follow the same pattern as the `server.json` file.

```bash
config set server.defaults.web.ruleFile=/path/to/rules.json
```

## Baked-in Rules

CommandBox has a few baked in rules that you can apply ad-hoc or as part of a server `profile`.

* **web.blockCFAdmin** - Returns 404 error page for any Adobe CF or Lucee Server admin administrator paths
* **web.blockConfigPaths** - Returns 404 error page for common config files such as `box.json` or `.env`

{% hint style="success" %}
If you want to customize the rules above, simply turn them off and include the rules directly in your `server.json` where you can modify them as you see fit.
{% endhint %}

## Block CF Admin

This setting has three possible settings:

* **true** - Block ALL access to ColdFusion and Lucee admins
* **false** - Do not block anything
* **external** - Only block access not coming from localhost. 

The exact rule activated when you set this property to `true` is:

```javascript
block-cf-admin()
```

The exact rule activated when you set this property to `external` is:

```javascript
cf-admin() -> block-external()
```

## Block Config Paths

This setting only allows `true`/`false`.  When set to `true`, the following rules are activated:

```javascript
// track and trace verbs can leak data in XSS attacks
disallowed-methods( methods={trace,track} )

// Common config files
regex(pattern='.*/(box.json|server.json|web.config|urlrewrite.xml|package.json|package-lock.json|Gulpfile.js|CFIDE/multiservermonitor-access-policy.xml|CFIDE/probe.cfm)', case-sensitive=false) -> set-error(404)

// Any file or folder starting with a period
regex('/\.')-> set-error( 404 )
```

