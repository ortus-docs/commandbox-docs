# Baked in Rules

## Baked-in Rules

CommandBox has a few baked in rules that you can apply ad-hoc or as part of a server `profile`.

* **web.blockCFAdmin** - Returns 404 error page for any Adobe CF or Lucee Server admin administrator paths
* **web.blockSensitivePaths** - Returns 404 error page for common config files such as `box.json` or `.env`
* **web.blockFlashRemoting -** Blocks all paths related to Flash and Flex remoting calls

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

## Block Sensitive Paths

This setting only allows `true`/`false`.  This is a bit of a catch-all rule for any sort of paths that a user should never be able to make on production that could reveal information or access secret parts of your CF installation. When set to `true`, the following rules are activated:

```javascript
// track and trace verbs can leak data in XSS attacks
disallowed-methods( methods={trace,track} )

// Common config files and sensitive paths that should never be accessed, even on development
regex( pattern='.*/(box.json|server.json|web.config|urlrewrite.xml|package.json|package-lock.json|Gulpfile.js)', case-sensitive=false ) -> set-error(404)

// Any file or folder starting with a period
regex('/\.')-> set-error( 404 )

// Additional serlvlet mappings in Adobe CF's web.xml
path-prefix( { '/JSDebugServlet','/securityanalyzer','/WSRPProducer' } ) -> set-error( 404 )

// java web service (Axis) files
regex( pattern='\.jws$', case-sensitive=false ) -> set-error( 404 )
```

When the `profile` is set to `production`, the following rule is also added which blocks the ColdFusion RDS access from CF Builder and access to TestBox runners:

```javascript
regex( pattern='.*/(CFIDE/multiservermonitor-access-policy.xml|CFIDE/probe.cfm|CFIDE/main/ide.cfm|tests/runner.cfm|testbox/system/runners/HTMLRunner.cfm)', case-sensitive=false ) -> set-error(404)
```

If you need to legitimately access any of these paths, you'll need to turn off this setting and custom-add the rules that you want to keep.  This setting is a convenience that adds several rules at once.

## Block Flash Remoting

This setting only allows `true`/`false`.  When set to `true`, the following rules are activated:

```javascript
// These all map to web.xml servlet mappings for ACF
path-prefix( { '/flex2gateway','/flex-internal','/flashservices/gateway','/cfform-internal','/CFFormGateway' } ) -> set-error( 404 )

// Files used for flash remoting
regex( pattern='\.(mxml|cfswf)$', case-sensitive=false ) -> set-error( 404 )
```

If you need to legitimately access any of these paths, you'll need to turn off this setting and custom-add the rules that you want to keep.  This setting is a convenience that adds several rules at once.

## Override just part of a baked-in rule

What if you want to go one step deeper? For instance, the `blockSensitivePaths` setting blocks a whole bunch of stuff all in one shot.  An example might be wanting to open up JUST the RDS IDE integration for your developers, which funnels through the `/CFIDE/main/ide.cfm` path which you can see is blocked above.  

The solution to this is quite simple and is all based on the ORDER in which your rules are processed.  Built-in CommandBox rules are the last to process. This means that if you hijack the predicates with a custom rule FIRST, you can override the behavior of the built in rules.  So, if we want to allow access to the /CFIDE/main/ide.cfm path, we just need to add a custom rule that matches that path and then aborts the predicate chain so no further rules fire.

```bash
server set web.rules="['path(/CFIDE/main/ide.cfm)->done']" --append 
```

Which gives you the following `server.json`

```javascript
{
    "web":{
        "rules":[
            "path(/CFIDE/main/ide.cfm)->done"
        ]
    }
}
```

The `done` handler is what bypasses all subsequent rules for the request. 

{% hint style="danger" %}
Since your custom rules are processed BEFORE the built-in rules, that also means that you have the ability to accidentally bypass security rules by applying another rule that intercepts the request! By default a custom rule won't block subsequent rules from firing unless you rewrite the request or use the special `done` handler.
{% endhint %}



