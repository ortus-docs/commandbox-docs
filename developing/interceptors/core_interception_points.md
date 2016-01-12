# Core Interception Points

Here is a list of all the core interception points in CommandBox that you can listen to.  Some have `interceptData` that comes along with them, while others don't.  Remember, the `interceptData` struct is passed by reference.  This means modifying any values directly in that struct will affect how processing continues afterwards inside of CommandBox where those values are used.  



