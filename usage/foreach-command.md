# forEach Command

The `foreach` command will execute another command against every item in an incoming list. The list can be passed directly or piped into this command. The default delimiter is a new line so this works great piping the output of file listings directly in, which have a file name per line.

This powerful construct allows you to perform basic loops from the CLI over arbitrary input.  
Most of the examples show file listings, but any input can be used that you want to iterate over.

This example will use the echo command to output each filename returned by ls. The echo is called  
once for every line of output being piped in.

```text
ls --simple | forEach
```

The default command is "echo" but you can perform an action against the incoming list of items.  
This example will use "cat" to output the contents of each file in the incoming list.

```text
ls *.json --simple | forEach cat
```

You can customize the delimiter. This example passes a hard-coded input and spits it on commas.  
So here, the install command is run three times, once for each package. A contrived, but effective example.

```text
forEach input="coldbox,testbox,cborm" delimiter="," command=install
```

If you want a more complex command, you can choose exactly where you wish to use the incoming item  
by referencing the default system setting expansion of ${item}. Remember to escape the expansion in   
your command so it's resolution is deferred until the forEach runs it internally.  
Here we echo each file name followed by the contents of the file.

```text
ls *.json --simple | foreach "echo \${item} && cat \${item}"
```

You may also choose a custom placeholder name for readability.

```text
ll *.json --simple | foreach "echo \${filename} && cat \${filename}" filename
```

Here's an example that has several things going on.  It takes a list of globbing patterns to get all CFC and CFM files recursively in a directory, then it loops over each file path, outputting it preceded by the current working directory, with backslashes replaced with forward slashes.  

```bash
dir **.cfc,**.cfm --simple | foreach "echo `pwd | #listchangedelims / \\`/\${item}"
```



### Iterating over JSON

The `forEach` can also iterate over JSON representations of objects or arrays.  This means you can pipe in JSON from a file, a command such as `package show` or any REPL operation that returns complex data.  The `delimiter` parameter is ignored for JSON input.

```text
package show dependencies | foreach
```

If iterating over an array, each item in the array will be available as `${item}`.  If iterating over a object, the object keys will be in `${item}` and the values will be in `${value}`.    You can customize the system setting name for value with the `valueName` parameter to `forEach`. 

```text
package show dependencies | foreach command="echo 'You have \${package} version \${version} installed'" itemName=package valueName=version
```





