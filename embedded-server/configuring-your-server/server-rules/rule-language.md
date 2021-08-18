# Rule Language

CommandBox's server rules are based directly on the "Predicate Language" feature of JBoss Undertow.  All of the official documentation for Undertow's predicate language applies to CommandBox. CommandBox doesn't limit the power of what Undertow provides at all.  In fact, we give you some additional out-of-the box predicates and handlers you can use.

There are three concepts you need to understand and you'll be writing your own rules in no time.

* **Handler** - A wrapper around the request that takes some sort of action such as returning a status code or rewriting the request.
* **Predicate** - A conditional that evaluates to true or false.  Used to Conditionally apply a handler
* **Exchange Attribute** - An abstracted way to refer to any piece of information about the request or response.  Can include URL, query string, cookies, headers, or CGI variables.

Since the parsing of your rules is handled by Undertow, it is **case sensitive** in many areas and CommandBox cannot control this.  The following must always be **lower case**:

* Predicate names
* Handler names
* Parameter names
* Keywords like `and`, `or`, `else`

The full undertow docs can be found here: [https://undertow.io/undertow-docs/undertow-docs-2.0.0/](https://undertow.io/undertow-docs/undertow-docs-2.0.0/#built-in-handlers-2)

## Handler

A handler will take action on the request. This can redirect the request, rewrite it, abort it, or modify attributes of the request such as setting an HTTP header or status code. If the predicate is omitted, the handler will always fire for all requests. Ex:

```javascript
disallowed-methods(trace)
```

### Common handlers:

* **set\(\)** - Sets an exchange attribute \(see below\)
* **rewrite\(\)** - Rewrite the request URL
* **redirect\(\)** - Redirect to another URL
* **header\(\)** - Sets an HTTP response header
* **set-error\(\)** - Set HTTP response code and returns the corresponding error page
* **ip-access-control\(\)** - Configure IP-based allow/deny rules \(wildcards and IP ranges allowed\)
* **done** - Skips remaining rules
* **request-limit\(\)** - Limits concurrent requests
* **restart** - Restarts process back at start of predicate rule chain \(combine with rewrite, etc\)
* **reverse-proxy\(\)** - Creates a round robin reverse proxy to a list of URLs
* **allowed-methods\(\)** / **disallowed-methods\(\)** - Enforces whitelist/blacklist of HTTP methods on current request
* Full list here: [https://undertow.io/undertow-docs/undertow-docs-2.0.0/\#built-in-handlers-2](https://undertow.io/undertow-docs/undertow-docs-2.0.0/#built-in-handlers-2)

A handler is called using the “name” from the docs and passing any required parameters. Params can be named or positional \(if there is only one\). Quoting values is only required if they have a space, comma or brace.  The following are all equivalent:

```javascript
set-error(404)
set-error("404")
set-error(response-code=404)
set-error(response-code="404")
```

Handler parameters that accept an array use the Java array literal syntax which is a comma-delimited list of values in curly braces.

```javascript
reverse-proxy( { 'http://host.com', 'http://host2.com' } )
```

More than one handler can be run by wrapping them in curly braces and using semicolons between them

```javascript
path(/restart) -> { rewrite(/foo/a/b); restart; }
```

A chain of handlers can be configured for the true AND false evaluation of a single predicate using the “else” keyword.

```javascript
regex('(.*).css') -> set(attribute='%{o,css}', value='true') else set(attribute='%{o,css}', value='false');
```

## Predicates

A predicate is a condition that must be matched for the handler to kick in. 

### Common predicates:

* **path\(\)** - match the incoming request path
* **path-suffix\(\)** - match full file/folder names at the end of the path like file.cfm
* **path-prefix\(\)** - match full file/folder names at the start of the path like /lucee/admin
* **method\(\)** - Match the HTTP method
* **regex\(\)** - Match a regular expression against any exchange attribute
* **equals\(\)** / contains\(\) - Do a text compare on any exchange attribute
* **path-template\(\)** - match a path with placeholders like `/foo/{bar}` and the placeholders become variables for use in later predicates or handlers in the chain. 
* Full list here: [https://undertow.io/undertow-docs/undertow-docs-2.0.0/\#textual-representation-of-predicates](https://undertow.io/undertow-docs/undertow-docs-2.0.0/#textual-representation-of-predicates)

A predicate is called by placing parentheses after the name and passing in the required arguments as specified in the docs for that predicate. Quoting values is only required if they have a space, comma or brace.   The following are all equivalent:

```javascript
method(GET)
method("GET")
method(value=GET)
method(value="GET")
```

Complex conditions can be constructed with more than one predicate using the keywords “**and**” or “**or**”.

```javascript
path-prefix(/tests/) OR path-prefix(/workbench)
```

A predicate can be negated with the keyword “**not**”

```javascript
not method(POST)
```

## Exchange Attributes

Exchange attributes are an abstraction Undertow provides to reference any part of the HTTP request or response. There is a textual placeholder that can be used in predicates and handlers that will be expanded in-place. 

### Common Exchange Attributes:

* **%{REMOTE\_IP}** - Remote IP address
* **%{PROTOCOL}** - Request protocol
* **%{METHOD}** - Request method
* **%{REMOTE\_USER}** - Remote user that was authenticated
* **%{RESPONSE\_TIME}** - Time taken to process the request, in ms
* **%{c,cookie\_name}** - Any cookie value
* **%{q,query\_param\_name}** - Any query parameter
* **%{i,request\_header\_name}** - Any request header
* **%{o,response\_header\_name}** - Any response header
* **${anything-here}** - Any value from the predicate context \(such as regex capture groups or path-template placeholders\)
* Full list here: [https://undertow.io/undertow-docs/undertow-docs-2.0.0/\#exchange-attributes-2](https://undertow.io/undertow-docs/undertow-docs-2.0.0/#exchange-attributes-2)

Use an exchange attribute like you would a variable as the parameter to a handler or predicate.

```javascript
set(attribute='%{o,someHeader}', value=myValue)
not equals('%{REMOTE_IP}', 127.0.0.1) -> set-error(403)
```

{% hint style="info" %}
Attributes, Cookie, query params, request headers, and response header names are not case sensitive. 
{% endhint %}

These are all equivalent:

```javascript
regex(pattern="/(MSIE 6\.0)", value="%{i,user-agent}" ) -> response-code( 404 )
regex(pattern="/(MSIE 6\.0)", value="%{i,USER-AGENT}" ) -> response-code( 404 )
regex(pattern="/(MSIE 6\.0)", value="%{i,UsEr-AgEnT}" ) -> response-code( 404 )
```



