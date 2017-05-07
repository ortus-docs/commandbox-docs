# TestBox

## testbox

**object**

This object stored configuration information used by the TestBox BDD and xUnit testing library.  The data is accessed by commands in the `testbox` command namespace.

### testbox.runner

**string** or **array**

`testbox.runner` can be a simple string that contains the full runner URL.

```javascript
"testbox" : {
    "runner" : "http://localhost/tests/runner.cfm"
}
```

```bash
package set testbox.runner="http://localhost/tests/runner.cfm"
package show testbox.runner
testbox run
```

`testbox.runner` can alternatively be an array of objects containing "named" runner URLs.


```javascript
"testbox" : {
    "runner" : [
        { "cf11"   : "http://cf9.localhost/tests/runner.cfm" },
        { "lucee" : "http://railo.localhost/tests/runner.cfm" }
    ]
}
```

```bash
package set testbox.runner="[ { default : 'http://localhost/tests/runner.cfm' } ]" --append
package show testbox.runner
testbox run default
```

## More...

Our box.json template shows other placeholder properties inside the `testbox` object, but only `runner` is implemented for now.  

*In the future, the `testbox` object may be moved into a separate JSON file for organizational purposes.*






