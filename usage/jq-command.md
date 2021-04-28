# jq command

JSON Query command for filtering data out of a JSON Object, file, or URL. jq is a query language built specifically for interacting with JSON type data. More information can be found at [https://jmespath.org/](https://jmespath.org/) as well as an online version to test your query Pass or pipe the text to process or a filename

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

`@` - Current Node \(eg. current number/string/array/object\) used to evaluate or check value

`&` - Expression \(Function or Keyname\) \(eg. &to\_number\(\) or &keyname\)

`!` - NOT Expression

`&&` - AND expression

`||` - OR expression

`` `{'ab':true}` `` - Literal Expressions \(this will be converted to json\)

`'foo'` - Raw String Literals not evaluated \(Single Quotes\)

## Available Functions

### Generic Functions

length, reverse, type, not\_null
```bash
CommandBox> jq [1,2,3,4,5,6,7,8,9] length(@)
=> 9
```

### Conversion Functions

to\_list, to\_array, to\_string, to\_number
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

`avg`, `first`, `join`, `last`, `matches`, `min`, `max`, `reverse`, `sum`, `sort`, `split`, `unique/uniq`
```bash
CommandBox> jq [1,2,-3,4,-5,6,-7,-8,9] avg(@)
-0.111111111111
```
### Struct or Array of Structs functions

`defaults`, `from_entries`, `group_by`,  `key_contains`, `keys`, `map`, `max_by`, `merge`, `min_by`, `omit`, `pluck`, `sort_by`, `to_entries`, `values`
```bash
# jsonfile.json
[
     {
        "logdir":"logs/aa",
        "Size":308
    },
    {
        "logdir":"logs/bb",
        "Size":303
    }
]

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
