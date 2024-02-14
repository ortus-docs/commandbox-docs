# Rule Examples

There are endless combinations of predicates and handlers you can apply to your apps. Here's an assortment of examples to get you going. Many of these are contrived, but are just mean to show the range of syntax and functionality.

Rewrite a URL:

```javascript
path('/healthcheck') -> rewrite('/healthcheck.cfm')
```

Stop processing rules for a given request:

```javascript
path(/healthcheck) -> done
path(/healthcheck) -> response-code(503)  // This rule is skipped
```

For all GET requests, set a response header called `type` with a value of `get`

```javascript
method(GET) -> set(attribute='%{o,type}', value=get)
```

If the incoming path ends with `.css` it will rewrite the request to `.xcss` and set a response header called `rewritten` to `true`.

```javascript
regex('(.*).css') -> { rewrite('${1}.xcss'); set(attribute='%{o,rewritten}', value=true) } 
```

Redirect all jpg requests to the same file name but with the png extension. The `${1}` exchange attribute is used to reference the first regex group.

```javascript
regex-nocase('(.*).jpg$') -> redirect('${1}.png')
```

Set a request header for all requests that you can access in your CFML app just like a normal HTTP header.

```javascript
set(attribute='%{i,someHeader}', value=someValue)
```

Set a Cache-Control response header only for default index pages(ie: index.html or index.cfm) in any folder.

```javascript
regex('.*/index\..*') or path-suffix('/') -> set(attribute='%{o,Cache-Control}', value='no-cache')
```

Set a response header for a specific file.

```javascript
path('/freshcontent.html') -> set(attribute='%{o,Cache-Control}', value='no-cache')
```

Match certain SES-style URLs and store the place holders (referenced as exchange attributes) into HTTP request headers.

```javascript
path-template('/product/{productid}') -> set(attribute='%{i,productid}', value='${productid}')
```

In this example, hitting the server with `/restart` skips the first rule, the second rule rewrites the request and then restarts back at the first rule which fires the second time through.

```javascript
path(/foo/a/b) -> response-code(503)
path(/restart) -> { rewrite(/foo/a/b); restart; }
```

Block access to a URL unless coming from a specific IP.

```javascript
path-prefix-nocase(/admin/) and not equals('%{REMOTE_IP}', 127.0.0.1) -> set-error( 404 )
```

For more control over IP address matches, use the `ip-access-control()` handler.

```javascript
path-prefix-nocase(value="/admin/") -> ip-access-control[default-allow=false, acl={'127.0.0.* allow'}, failure-status=404]
```

Leading slash is NOT required. Both of these rules are the same:

```javascript
path-nocase(box.json) -> set-error( 404 )
path-nocase(/box.json) -> set-error( 404 )
```

It is not required to include `.*`\_ at the end of a regex path unless you’ve set `full-match=true` \_

```javascript
regex-nocase( "^/tests/" ) -> set-error( 404 ) 
regex-nocase( pattern='^/tests/.*", full-match=true ) -> set-error( 404 )
```

Perform a regex a case insensitive search like so:

```javascript
regex(pattern="Googlebot", value="%{i,USER-AGENT}", case-sensitive=false ) -> set-error( 404 )
# or...
regex-nocase(pattern="Googlebot", value="%{i,USER-AGENT}" ) -> set-error( 404 )
```

When attribute values are not quoted, all leading and trailing whitespace will be trimmed. The following are all the same:

```javascript
path( "/foobar" )
path(/foobar)
path( /foobar )
```

But this example is different. The leading and trailing spaces will be preserved in the path string.

```javascript
path( " /foobar " )
```

Basic MVC rewrite:

```javascript
not regex-nocase( '.*\.(bmp|gif|jpe?g|png|css|js|txt|xls|ico|swf|cfm|cfc|html|htm)$' ) -> rewrite('/index.cfm/%{RELATIVE_PATH}')
```

Add a CORs header to every request

```javascript
set(attribute='%{o,Access-Control-Allow-Origin}', value='*')
```

Rewrite requests to a sub folder based on the domain

```javascript
equals( %{LOCAL_SERVER_NAME}, 'site1.com' ) -> rewrite( '/site1/%{REQUEST_URL}' )
equals( %{LOCAL_SERVER_NAME}, 'site2.com' ) -> rewrite( '/site2/%{REQUEST_URL}' )
equals( %{LOCAL_SERVER_NAME}, 'site3.com' ) -> rewrite( '/site3/%{REQUEST_URL}' )
```

Custom SES URL rewrite that turns 6 digit product code into a query string. (If this rule goes in your `server.json`, you'll need `\\` in front of the dollar sign to escape it twice. Once for the system setting expansion and once for the JSON file.)

```javascript
regex( '^/product/([A-Z]{6})$' ) -> rewrite( '/product.cfm?productID=${1}' )
```

Reject requests using an unknown host header.

```javascript
not equals-nocase( %{LOCAL_SERVER_NAME}, 'www.myDomain.com' ) -> set-error( 403 )
```

Create reverse proxy to multiple hosts

```json
path-prefix('/reports') -> load-balanced-proxy({'http://reports1.mydomain.com','http://reports2.mydomain.com'})
```

Create reverse proxy to a single host

```json
proxy( 'http://localhost:8085' )
```
