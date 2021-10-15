# Printing tables

The print helper contains a feature that will generate ASCII tables to print tabular data from the CLI.  The easiest way to see what this looks like is to run the `outdated` command in the root of a project:

![Table output](<../../.gitbook/assets/image (21).png>)

## Usage

There are several easy ways to call the table printer.  Here are the arguments to the `print.table()` method:

* `data` - Any type of data for the table. Each item in the array may either be an array in the correct order matching the number of headers or a struct with keys matching the headers.
* `includedHeaders` - A list of headers to include. Used for query inputs
* `headerNames` - An list/array of column headers to use instead of the default
* `debug` - Only print out the names of the columns and the first row values

Below are some basic examples that all produce the same CLI output:

![](<../../.gitbook/assets/image (5).png>)

### Array of arrays plus column headers

You can pass an array of column headers as the first argument and then an array of rows, where each row contains an array with data for each column.

```javascript
print.table(
	headerNames = [ 'First Name', 'Last Name' ],
	data = [
		[ 'Brad', 'Wood' ],
		[ 'Luis', 'Majano' ],
		[ 'Gavin', 'Pickin' ]
	]
);
```

### Array of structs plus column headers

You can also pass an array of structs where the keys of the structs match the column names in the headers array.

```javascript
print.table(
	headerNames = [ 'First Name', 'Last Name' ],
	data = [
		{ 'First Name' : 'Brad', 'Last Name' : 'Wood' },
		{ 'First Name' : 'Luis', 'Last Name' : 'Majano' },
		{ 'First Name' : 'Gavin', 'Last Name' : 'Pickin' }
	]
);
```

### Query Object

If you have data already in a query object, you can pass that directly as the first parameter.  The `data` parameter is ignored here.

```javascript
var qry = queryNew(
	'First Name,Last Name',
	'varchar,varchar',
	[
		[ 'Brad', 'Wood' ],
		[ 'Luis', 'Majano' ],
		[ 'Gavin', 'Pickin' ]
	]		
);

print.table( qry );
```

### Formatting cells

There is some basic support for applying formatting at a cell level.  To do this, use the "array" version of the data and instead of providing a string for the cell value, provide a struct containing:

* `value` - The actual string value to print
* `options` - A string of print helper options- typically a color or background color

The `options` can contain any color from the `system-colors` command and follows the same format as the [Print Helper](./).

```javascript
print.table(
	headerNames = [ 'First Name', 'Last Name' ],
	data = [
		[
			{ value : 'Brad', options : 'blueOnWhite' },
			'Wood'
		],
		[
			{ value : 'Gavin', options : 'whiteOnBlueViolet' },
			'Majano'
		],
		[
			{ value : 'Gavin', options : 'DeepPink1' },
			'Pickin'
		]
	]
);
```

![](<../../.gitbook/assets/image (20).png>)

### Include only certain columns

You can limit the columns that display in the table regardless of whether you use an array of data or a query object by using the `includeHeaders` parameter.  This example outputs a sorted list of the names and version of all the Lucee Extensions installed in the CLI. (`extensionList()` is a built in Lucee function that returns a query object)

```javascript
print.table(
	data = extensionList()
		.sort( (a,b)=>compare( a.name, b.name ) ),
	includeHeaders = 'name,version'
);
```

![](<../../.gitbook/assets/image (14).png>)

### Auto-resizing columns

If the data on a given row is too much to display in the given terminal, the table printer will automatically shrink columns based on how much "whitespace" is in them. Values which are too long to display will be truncated and `...` will appear at the end.  Here is the output of the `outdated` command.

![](<../../.gitbook/assets/image (7).png>)

### Removal of columns in small terminals

If a user has a terminal so small that all the columns simply won't fit, the table printer will automatically eliminate extra columns on the right hand side of the table and insert a single column whose header and values are all `...` to show missing data.  Here is the output of the `outdated` command.

![](<../../.gitbook/assets/image (4).png>)

