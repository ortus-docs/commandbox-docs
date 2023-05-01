# Printing Columns

If you have a large list of items you need to output, you can use the `print.columns()` helper.  Pass an array of simple values to be printed, and columns  will be created on the screen based on the widest item and the terminal width. &#x20;

```javascript
print.columns( directoryList( resolvePath( /lib ) ) )
```

You can also pass a UDF as the second argument which will be called for each item in the array and can return a string with additional formatting text for the print helper to format that item.  The closure receives the following arguments:

* item (string) being printed
* row number
* column number

```
print.columns(
    directoryList( resolvePath( /lib ) ),
    (item,row,col)=>item.endsWith('.txt') ? 'blue' : 'green'
)
```

This example would print out a list of files in the directory, coloring all text files blue, and the rest green.

This helper is the same as the `printColumns` command with the differences being the command accepts no formatting closure and will convert list data into an array or will accept a JSON array directly.

```
printColumns data="1,2,3,4,5" delimiter=","

# or ...
ls --simple | printColumns
```
