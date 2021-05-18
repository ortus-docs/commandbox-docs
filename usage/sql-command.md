# sql Command

There is a `sql` command which will accept any sort of data as JSON, marshal it into a query object and allow you to alias, filter, order, and limit the rows on the fly using CFML's query of queries.

## Parameters

* `data` - The JSON to process
* `select` - A SQL list of column names, eg. "name,version"
* `where` - A SQL where filter used in a query of queries, eg. "name like '%My%'"
* `orderby` - A SQL order by used in a query of queries, eg. "name asc, version desc"
* `limit` - A SQL limit/offset used in a query of queries, eg. "5" or "5,10" \(eg. offset 5 limit 10\)
* `headerNames` - An list of column headers to use \(used for array of arrays\)

When using an array of arrays and not specifying `headerNames`, the columns will be named `col_1`, `col_2`, `col_3`, etc...

## Examples

Filter, sort, limit, and select extensions installed into the CLI \(output as table\)

```bash
#extensionlist  | sql select=id,name where="name like '%sql%'" orderby=name limit=3 | printTable
```

Order and select JSON data from a file \(output as JSON\)

```bash
cat myfile.json | sql select=col1,col2 orderby=col2
```

Limit JSON \(output as table\)

```bash
sql data=[{a:1,b:2},{a:3,b:4},{a:5,b:6}] where="a > 1" | printTable
```

The `sql` command works very nicely with the `tablePrinter` command.

