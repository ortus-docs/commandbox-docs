# Task Target Dependencies

For a task that has more than one target \(method\) you can specify dependencies that will run, in the order specified, prior to your final target.  Specify task target dependencies as a comma-delimited list in a `depends` annotation on the target function itself.  There is no limit to how many target dependencies you can have, nor how deep they can nest.  

```javascript
component {

  function run() depends="runMeFirst" {
  }
  
  function runMeFirst() {
  }
  
}
```

Given the above Task Runner, typing

```bash
task run
```

would run the `runMeFirst()` and `run()` method in that order.

