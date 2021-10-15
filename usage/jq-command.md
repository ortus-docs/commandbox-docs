# jq Command

JSON Query command for filtering data out of a JSON Object, file, or URL. jq is a query language built specifically for interacting with JSON type data. More information can be found at [https://jmespath.org/](https://jmespath.org) as well as an online version to test your query Pass or pipe the text to process or a filename

## Using jq with a file

```bash
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

```bash
> jq "basic.json" "a"
=> "a": {
      "b": {
         "c": {
            "d": "value"
         }
      }
   }
```

```bash
> jq "basic.json" "a.b.c.d"
=> value
```

```bash
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

```bash
> jq "basic.json" "dan[1]"
=> 2
```

## Using jq with a URL

```bash
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

```bash
> jq '{"a": {"b": {"c": {"d": "value"}}}}' a.b.c.d
=> value
```

## Special Keys / Expressions

`@` - Current Node (eg. current number/string/array/object) used to evaluate or check value

`&` - Expression (Function or Keyname) (eg. \&to_number() or \&keyname)

`!` - NOT Expression

`&&` - AND expression

`||` - OR expression

`{'ab':true}` - Literal Expressions (this will be converted to json)

`'foo'` - Raw String Literals not evaluated (Single Quotes)

## Available Functions

### Generic Functions

length, reverse, type, not_null

```bash
CommandBox> jq [1,2,3,4,5,6,7,8,9] length(@)
=> 9
```

### Conversion Functions

to_list, to_array, to_string, to_number

```bash
CommandBox> jq [1,2,-3,4,-5,6,-7,-8,9] [].to_string(@)
> [
    "1",
    "2",
    "-3",
    "4",
    "-5",
    "6",
    "-7",
    "-8",
    "9"
]
```

## String / Number Functions

`abs`, `ceil`, `floor`

```bash
CommandBox> jq [1,2,-3,4,-5,6,-7,-8,9] [].abs(@)
=> [
    1,
    2,
    3,
    4,
    5,
    6,
    7,
    8,
    9
]
```

## Boolean Checks

`ends_with`, `starts_with`, `contains`

```bash
CommandBox> jq [1,2,-3,4,-5,6,-7,-8,9] [?contains('-8',@)]
=> [
    -8
]
```

#### All functions can be used in other functions with the "&" operator.

A common example would be getting a person with the highest or lowest networth `max_by(people, &abs(net_worth))`

### Array Functions

`avg: ( ARR )` - convert array of number to average (ex. \[1,2,3] -> 2)

`first: ( ARR/STR )` - convience method to get the first item

`group_by: ( ARR )` - Splits a collection into sets

`join: ( ARR, STR )` - concatenate an array of strings/numbers with a provided delimiter to a string

`last: ( ARR/STR )` - convience method to get the last item

`matches: ( STR/ARR, searchTerm )` - regex match string

`min: ( ARR )` - get the minimum string/number/dates of an array (ex. \[1,2,3] -> 1)

`max: ( ARR )` - get the maximum string/number/dates of an array (ex. \[1,2,3] -> 3)

`reverse: ( STR/ARR )` - returns a reversal of a string or array

`sum: ( ARR )` - convert array of number to sum (ex. \[1,2,3] -> 6)

`sort: ( STR_ARR/NUM_ARR )` - sorts an array of strings/numbers/dates

`split: ( ARR/STR, STR )` - splits strings into arrays

`unique/uniq: ( ARR )` - remove duplicates

```bash
CommandBox> jq [1,2,-3,4,-5,6,-7,-8,9] avg(@)
-0.111111111111
```

### Struct or Array of Structs functions

`defaults: ( OBJ/ARR, OBJ )` - sets default values if missing on **1 or more** structs

`key_contains ( OBJ, &KeyName )` - boolean check if struct contains key name

`from_entries ( OBJ/ARR )` - converts a `{type:orange}` -> `{key: type, value:orange}`

`keys: ( OBJ/ARR )` - returns an array of keys

`max_by: ( ARR,Function/Key )` - same as **min** but targets a key inside the array and returns **a single struct**

`merge: ( OBJ/ARR, ...)` - Merges objects into one single object **with overwrite**

`min_by: ( ARR,Function/Key )` - same as **max** but targets a key inside the array and returns **a single struct**

`omit ( OBJ/ARR, STR/ARR )` - loops over 1+ struct and excludes keys provided `to_pairs: ( OBJ/ARR )`- converts a `{type:orange}` -> `[[type, orange]]`

`pluck ( OBJ/ARR, STR/ARR )` - loops over 1+ struct and only includes keys provided

`sort_by: ( ARR, Function/Key )` - same as **sort** but targets a key inside the array and returns **the entire array**

`to_entries ( OBJ/ARR )` - converts a `{type:orange}` -> `{key: type, value:orange}`

`values: ( OBJ/ARR )` - returns an array of values

`map: ( Function/Key, ARR )` -

```bash
# jsonfile.json

CommandBox> jq box.json pluck(@,'name') # only show 'name' key

CommandBox> jq box.json omit(@,'name,version') # show all keys except 'name & version' keys

CommandBox> jq box.json keys(@) # list an array of all keys in a struct

CommandBox> jq box.json values(@) # list an array of all values in a struct

CommandBox> jq box.json to_entries(@)
=> [
  {
    "key":"author",
    "value":"Scott Steinbeck"
  },
  {
    "key":"bugs",
    "value":""
  },
  {
    "key":"changelog",
    "value":""
  }...


Commandbox> jq box.json key_contains(@,'on')
=> {
  "shortDescription":"",
  "instructions":"",
  "version":"0.0.0",
  "location":"ForgeboxStorage",
  "documentation":"",
  "contributors":[],
  "description":""
}

# filter the server list and then group by the value of the status key
Commandbox>  server list --json | jq "group_by([].{name: name, status: status},'status')" 

# Can also be written with pipes
# Step 1. filter server list array to just name and status keys
# JQ Pipe to next function
# Step 2. group by @ (each item) where the value of 'status' is the same
Commandbox> server list --json | jq "[].{name: name, status: status} | group_by(@,'status')" 

=> {
  "running":[
    {
      "status":"running",
      "name":"Server 1"
    }
  ],
  "stopped":[
    {
      "status":"stopped",
      "name":"Server 2"
    },...

CommandBox> jq jsonfile.json sort_by(@,&Size)
=> [
    {
        "logdir":"logs/bb",
        "Size":303
    },
    {
        "logdir":"logs/aa",
        "Size":308
    }
]
```
