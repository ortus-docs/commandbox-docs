# What's New in 5.3.1

### Updated bundled Java libraries

* **JLine** - 3.19.0
* **jGit** - 5.11.0.202103091610-r
* **Launch4j** - 3.14
* **JANSI** - 2.3.2 

### Recursive jar scanning libDirs

When you configure libDirs for a server, CommandBox used to only load jar files found in the root.  Now it will include sub directories which gives you more flexibility around how to install your jars.

```bash
# Jar installs to lib/jline-3.0.0.M1/jline-3.0.0.M1.jar
install "jar:https://search.maven.org/remotecontent?filepath=jline/jline/3.0.0.M1/jline-3.0.0.M1.jar"

# Load up the jar when the server starts
server set app.libDirs=lib
```

### New printTable Command

In the previous release, we introduced a new helper for printing ASCII Art tables in your custom commands and task runners.  We've taken this a step further and wrapped the table printer utiilty in a new command so you can use it from the CLI directly.  We've also expanded its functionality to accept ANY data in as JSON and it will marshall it into a query for you. This means it can be a query, an array of structs, an array or arrays, and more. You can now get quick and easy visualization of any data right from the CLI or in builds.

```bash
# array of structs
printTable [{a:1,b:2},{a:3,b:4},{a:5,b:6}]

╔═══╤═══╗
║ a │ b ║
╠═══╪═══╣
║ 1 │ 2 ║
╟───┼───╢
║ 3 │ 4 ║
╟───┼───╢
║ 5 │ 6 ║
╚═══╧═══╝

# array of arrays
printTable data=[[1,2],[3,4],[5,6]] headerNames=foo,bar

╔═════╤═════╗
║ foo │ bar ║
╠═════╪═════╣
║ 1   │ 2   ║
╟─────┼─────╢
║ 3   │ 4   ║
╟─────┼─────╢
║ 5   │ 6   ║
╚═════╧═════╝

# Query object
#extensionlist | printTable name,version

╔═════════════════════════════════════════╤═══════════════════╗
║ name                                    │ version           ║
╠═════════════════════════════════════════╪═══════════════════╣
║ MySQL                                   │ 8.0.19            ║
╟─────────────────────────────────────────┼───────────────────╢
║ Microsoft SQL Server (Vendor Microsoft) │ 6.5.4             ║
╟─────────────────────────────────────────┼───────────────────╢
║ PostgreSQL                              │ 9.4.1212          ║
╟─────────────────────────────────────────┼───────────────────╢
║ Ajax Extension                          │ 1.0.0.3           ║
╚═════════════════════════════════════════╧═══════════════════╝

# JSON list of all servers
server list --json | printTable name,host,port,status

╔══════════════════════════════╤═════════════════════════════╤═══════╤═════════╗
║ name                         │ host                        │ port  │ status  ║
╠══════════════════════════════╪═════════════════════════════╪═══════╪═════════╣
║ servicetest                  │ 127.0.0.1                   │ 54427 │ stopped ║
╟──────────────────────────────┼─────────────────────────────┼───────┼─────────╢
║ servicetest2                 │ 127.0.0.1                   │ 52919 │ stopped ║
╟──────────────────────────────┼─────────────────────────────┼───────┼─────────╢
║ FRDemos                      │ 127.0.0.1                   │ 50458 │ stopped ║
╚══════════════════════════════╧═════════════════════════════╧═══════╧═════════╝
```

### New sql command for on-the-fly manipulation of data

As if the previous command isn't cool enough, we've also added a new "sql" command which will also accept any sort of data as JSON, marshall it into a query object and allow you to alias, filter, order, and limit the rows on the fly using CFML's query of queries!

```bash
# filter, sort, limit, and select extensions installed into the CLI (output as table)
#extensionlist  | sql select=id,name where="name like '%sql%'" orderby=name limit=3 | printTable

# order and select JSON data from a file (output as JSON)
cat myfile.json | sql select=col1,col2 orderby=col2

# limit JSON (output as table)
sql data=[{a:1,b:2},{a:3,b:4},{a:5,b:6}] where="a > 1" | printTable

```

The **sql** command works very nicely with the new **tablePrinter** command, and truly makes JSON a first class citizen of the CommandBox CLI.

### Small change to print.table\(\) helper

We've made a small adjustment to the **print.table\(\)** helper that was introduced in CommandBox 5.3.1 as follows.  The old method signature is

```javascript
public string function print(
	required any headers,
	array data=[],
	string includeHeaders        
) {
```

And the new method signature is:

```javascript
public string function print(
	required any data=[],
	any includedHeaders="",
	any headerNames="",
	boolean debug=false
) {
```

The parameters to the new **printTable** command matches the NEW method signature of the **print.table\(\)** helper as well.

### Format XML in REPL

When working with XML in the REPL, formatting is now applied when the XML is printed out to the console, making it easier to read \(same as JSON\)

```text
❯ repl "XMLParse( '<root><brad>wood</brad></root>' )"

<?xml version="1.0" encoding="utf-8"?><root>
  <brad>wood</brad>
</root>
```

## Release Notes

### Bug

[COMMANDBOX-1320](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1320) Server stop doesn't message user when it fails

[COMMANDBOX-1319](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1319) Stop loading cfusion/lib in system class loader

[COMMANDBOX-1318](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1318) 5.3.0 errors with commandbox-dotenv 1.x versions due to WireBox change

[COMMANDBOX-1314](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1314) When building Lucee war from local jars, seeded web.xml file is ignored

[COMMANDBOX-1311](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1311) Table printer error with no rows

[COMMANDBOX-1310](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1310) update '{slug}' fails as it is trying to print the package version and its dependencies.

[COMMANDBOX-1308](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1308) Relative Web Alias Behavior \(regression\)

[COMMANDBOX-1307](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1307) jq doesn't resolve file paths to current working directory

[COMMANDBOX-1303](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1303) CFEngine adobe - Could not initialize class coldfusion.vfs.VFile when using s3 protocol

### Improvement

[COMMANDBOX-1328](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1328) Improve performance of piping large strings to "cfml" command

[COMMANDBOX-1317](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1317) Format XML in REPL

[COMMANDBOX-1316](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1316) Change default CLI JSON representation of query to array of structs

[COMMANDBOX-1306](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1306) Allow upgrade command to pull stable versions when CLI is a prerelease version

[COMMANDBOX-1305](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1305) Update bundled java libraries

[COMMANDBOX-1302](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1302) app.libDirs does not load jars/classes recursively from sub folders

[COMMANDBOX-1210](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1210) Allow for relative URLs when defining trayoption elements

[COMMANDBOX-1194](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1194) Add Libraries To Runwar Necessary For URLRewrite Proxy

### New Feature

[COMMANDBOX-1324](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1324) New "printTable" command to add CLI usage of table printer

[COMMANDBOX-1323](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1323) New "sql" command to filter tabular data with SQL

[COMMANDBOX-1321](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1321) Add --verbose to 'server stop' to see raw output

[COMMANDBOX-1309](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1309) Add printTable command that proxies to print.table\(\) helper

