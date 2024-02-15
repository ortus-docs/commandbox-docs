# Rewrites Map

If you've used `mod_rewrite` with Apache web server, you may be familiar with its Rewrite Map functionality, which allows you to create dynamic rewrites based on a text file of mappings.  We have a similar feature built on Undertow's Server Rules that mimics Apache's feature.

{% hint style="info" %}
Note, when in Mult-Site mode, each site has its own rewrite maps, even if they share the same name.  There is no overlap between sites!
{% endhint %}

## Declare a Rewrite Map

There is a `rewrite-map()` handler which declares a named map file.  It accepts an absolute path to the rewrite file, and case sensitivity flag.

```json
{
  "web" : {
    "rules":[
      "rewrite-map( name=myMap, file='/path/to/myMap.txt' case-sensitive=false )"
    ]
  }
}
```

The file format matches that of Apache's rewrite map functionality. &#x20;

* Lines starting with `#` are a comment
* empty lines are ignored
* First space-delimited token becomes the key
* All remaining tokens become the value
* Lines with a single token default the value to an empty string

Here is an example file:

```
#comment

brad wood
luis majano
jorge reyes bendeck
RACHEL                           WOOD
sdf sdf
```

### Watch disk for changes

If the date modified on the rewrite map file changes on disk, the data will be automatically re-loaded into memory.

## Using the Rewrite Map

There are two handler/predicates you can use to interact with the rewrite map. Make sure these are in your list of Server Rules AFTER the map definition.

### Check if a key exists

There is a `rewrite-map-exists()` predicate which will tell you if a given key exists in the map (Apache doesn't have this)

```javascript
rewrite-map-exists( map='myMap', key='%{RELATIVE_PATH}' )
```

You can pair this predicate to only use the rewrite map if there is a match.

### Get the value for a key from the map

There is a new `%{map:name-name:mapKey|defaultValue}` exchange attribute which mostly follows Apache's syntax.  The only limitation is nested exchange attributes must use \[] instead of {} due to an [Undertow parsing issue](https://issues.redhat.com/browse/UNDERTOW-2273)).

```json
{
  "web" : {
    "rules":[
      "rewrite-map( name=myMap, file='/path/to/myMap.txt' case-sensitive=false )",
      "regex-nocase( '^/foo/(.*)$' ) -> rewrite( 'index.cfm?page=%{map:myMap:$[1]|99}' )"
    ]
  }
}
```

In the example above, we're using a regex predicate to extract the text after `/foo/` in the URL and then we reference that capture group as `$[1]` which we pass in as the key to the map.  If there is no key in the map for that value, we default to `99`.

So, given our example rule map file above, the url

```
http://site1.com/foo/luis
```

would be rewritten to

```
http://site1.com/index.cfm?page=majano
```
