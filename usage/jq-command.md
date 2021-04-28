# jq command
JSON Query command for filtering data out of a JSON Object, file, or URL. jq is a query language
built specifically for interacting with JSON type data. More information can be found
at https://jmespath.org/ as well as an online version to test your query
Pass or pipe the text to process or a filename
## Using jq with a file 
``` 
# basic.json
   "a": {
      "b": {
         "c": {
            "d": "value"
         }
      }
   },
   "dan": [1,2,3,4,5,6,7,8],
   "ban": "bar",
   "cat": "baz"
}
```
```
> jq "basic.json" "a"
=> "a": {
      "b": {
         "c": {
            "d": "value"
         }
      }
   }
```
```
> jq "basic.json" "a.b.c.d"
=> value
```
```
> jq "basic.json" "dan"
=> [
    1,
    2,
    3,
    4,
    5,
    6,
    7,
    8
]
```
```
> jq "basic.json" "dan[1]"
=> 2
```
## Using jq with a URL
```
> jq "https://official-joke-api.appspot.com/jokes/ten" [].join('.....',[setup,punchline])
=> [
    "Lady: How do I spread love in this cruel world?.....Random Dude: [...\ud83d\udc98]",
    "Why are skeletons so calm?.....Because nothing gets under their skin.",
    "How come a man driving a train got struck by lightning?.....He was a good conductor.",
    "What do you call a pig with three eyes?.....Piiig",
    "How do you steal a coat?.....You jacket.",
    "Did you hear about the submarine industry?.....It really took a dive...",
    "What does a pirate pay for his corn?.....A buccaneer!",
    "A programmer puts two glasses on his bedside table before going to sleep......A full one, in case he gets thirsty, and an empty one, in case he doesn’t.",
    "How did the hipster burn the roof of his mouth?.....He ate the pizza before it was cool.",
    "Did you hear the news?.....FedEx and UPS are merging. They’re going to go by the name Fed-Up from now on."
]
```
## Using jq with inline json
```
> jq '{"a": {"b": {"c": {"d": "value"}}}}' a.b.c.d
=> value
```

### Special Keys / Expressions
`@` - Current Node \(eg. current number/string/array/object\) used to evaluate or check value
`&` - Expression \(Function or Keyname\) \(eg. &to\_number\(\) or &keyname\)
`!` - NOT Expression
`&&` - AND expression
`||` - OR expression
``\`{'ab':true}\```  - Literal Expressions \(this will be converted to json\)
`'foo'` - Raw String Literals not evaluated \(Single Quotes\)
## Available Functions
### Argument Types
`STR` \( String \), `NUM` \( Number \), `ARR` \( Array \), `OBJ` \( Object/Struct \)
### Generic Functions
`length: ( STR/ARR/OBJ )` - Count the number of items
`reverse: ( STR/ARR )` - returns a reversal of a string or array
`type: ( ANY )` - returns the type of value interpreted by JMESPATH
`not_null: ( ANY,... )` - Returns the first argument that does not resolve to null.
### Conversion Functions
`to_list: ( ARR, STR )` - Convert an array to a string list with a provided delimiter
`to_array: ( ANY )` - Wraps the whole value in an array
`to_string: ( ANY )` - Convert whole value to a string
`to_number: ( ANY )` - Converts whole value to a number
## String / Number Functions
`abs: ( NUM )` - convert number to absolute value \(ex. -2 -&gt; 2 \)
`ceil: ( NUM )` - convert number to ceiling \(ex. 2.3 -&gt; 3\)
`floor: ( NUM )` - convert number to floor \(ex. 2.8 -&gt; 2\)
## Boolean Checks
`ends_with: ( STR,STR )` - returns true if string ends with provided value
`starts_with: ( STR,STR )` - returns true if string start with provided value
`contains: ( STR,ANY )` - returns true if string contains with provided value
#### All functions can be used in other functions with the "&" operator.
A common example would be getting a person with the highest or lowest networth `max_by(people, &abs(net_worth))`
### Array Functions
`avg: ( ARR )` - convert array of number to average \(ex. \[1,2,3\] -&gt; 2\)
`first: ( ARR/STR )` - convience method to get the first item
`group_by: ( ARR )` - Splits a collection into sets
`join: ( ARR, STR )` - concatenate an array of strings/numbers with a provided delimiter to a string
`last: ( ARR/STR )` - convience method to get the last item
`matches: ( STR/ARR, searchTerm )` - regex match string
`min: ( ARR )` - get the minimum string/number/dates of an array \(ex. \[1,2,3\] -&gt; 1\)
`max: ( ARR )` - get the maximum string/number/dates of an array \(ex. \[1,2,3\] -&gt; 3\)
`reverse: ( STR/ARR )` - returns a reversal of a string or array
`sum: ( ARR )` - convert array of number to sum \(ex. \[1,2,3\] -&gt; 6\)
`sort: ( STR_ARR/NUM_ARR )` - sorts an array of strings/numbers/dates
`split: ( ARR/STR, STR )` - splits strings into arrays
`unique/uniq: ( ARR )` - remove duplicates

### Struct or Array of Structs functions

`defaults: ( OBJ/ARR, OBJ )` - sets default values if missing on **1 or more** structs
`key_contains ( OBJ, &KeyName )` - boolean check if struct contains key name
`from_entries ( OBJ/ARR )` - converts a `{type:orange}` -&gt; `{key: type, value:orange}`
`keys: ( OBJ/ARR, )` - returns an array of keys
`max_by: ( ARR,Function/Key )` - same as **min** but targets a key inside the array and returns **a single struct**
`merge: ( OBJ/ARR, ...)` - Merges objects into one single object **with overwrite**
`min_by: ( ARR,Function/Key )` - same as **max** but targets a key inside the array and returns **a single struct**
`omit ( OBJ/ARR, STR/ARR )` - loops over 1+ struct and excludes keys provided `to_pairs: ( OBJ/ARR )`- converts a `{type:orange}` -&gt; `[[type, orange]]`
`pluck ( OBJ/ARR, STR/ARR )` - loops over 1+ struct and only includes keys provided
`sort_by: ( ARR, Function/Key )` - same as **sort** but targets a key inside the array and returns **the entire array**
`to_entries ( OBJ/ARR )` - converts a `{type:orange}` -&gt; `{key: type, value:orange}`
`values: ( OBJ/ARR )` - returns an array of values
`map: ( Function/Key,ARR )` -

