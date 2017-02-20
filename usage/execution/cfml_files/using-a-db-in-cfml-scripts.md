# Using a DB in CFML Scripts

One common question is how to access the database from one of these scripts.  Your code is executed on Lucee Server \(version 4.5 at the time of this writing\) which is the version of Lucee that the core CLI runs on.   The CLI has the full power of a Lucee server running under the covers, but there's no web-based administrator for you to acess to do things like adding datasources for your scripts to use.  It would considered poor form anyway since standalone scripts are best if they're self-contained and don't have external dependencies like server settings necessary to run.

## Lucee allows datasource to be a struct

So the easiest way to accomplish this is simply to exploit a little known but very cool feature of Lucee that allows the datasource attribute of most tags to be not only a string which contains the name of the datasource, but also a struct that contains the \_definitiion\_ of the datasource.  This will create an on-the-fly connection to your database without any server config being necessary which is perfect for a stand-alone script.  Here is what that looks like.  Note, I'ms using queryExecute\(\), but it would work just as well in a cfquery tag.



```js
ds = {
  class: 'org.gjt.mm.mysql.Driver',
  connectionString: 'jdbc:mysql://localhost:3306/bradwood?useUnicode=true&characterEncoding=UTF-8&useLegacyDatetimeCode=true',
  username: 'root',
  password: 'encrypted:bc8acb440320591185aa10611303520fe97b9aa92290cf56c43f0f9f0992d88ba92923e215d5dfd98e632a27c0cceec1091d152cbcf5c31d'
};

var qry = queryExecute( sql='select * from cb_role', options={ datasource : ds } );

for( var row in qry ) {
  echo( row.role & chr( 10 ) );
}

```

So, the first block simply declares a struct that represents a datasource connection.  Then I use that struct as my datasource.  You might be thinking, "where the heck did he get that struct??".  Glad you asked.  Start up a Lucee 4 server, edit a datasource that has the connection properties you want and then at the bottom of the edit page you'll see a code sample you can just copy and paste from.  This is the code for an \`Application.cfc\`, but you can re-use the same struct here.

!\[Get DS definition from the Lucee administrator\]\([https://www.ortussolutions.com//\\_\\_media/datasource-lucee-definition.png\](https://www.ortussolutions.com//\_\_media/datasource-lucee-definition.png\)\)

## Another method

There are a couple tags inside Lucee that don't support this just yet.  \`&lt;CFDBInfo&gt;\` is one of them.  \[Ticket Here\]\([https://luceeserver.atlassian.net/browse/LDEV-1026\](https://luceeserver.atlassian.net/browse/LDEV-1026\)\)  In this case, you need a "proper" datasource defined that you can reference by name.  Lucee has some more tricks up its sleeve for this.  You can simulate the same thing that happens when you add a datasource to your \`Application.cfc\` with the following code.  This will define a datasource for the duration of the time the CLI is running in memory, but it will be gone the next time you start the CLI.

```js
appSettings = getApplicationSettings();
dsources = appSettings.datasources ?: {};

dsources[ 'myNewDS' ] = {
    class: 'org.gjt.mm.mysql.Driver',
    connectionString: 'jdbc:mysql://localhost:3306/bradwood?useUnicode=true&characterEncoding=UTF-8&useLegacyDatetimeCode=true',
    username: 'root',
    password: 'encrypted:bc8acb440320591185aa10611303520fe97b9aa92290cf56c43f0f9f0992d88ba92923e215d5dfd98e632a27c0cceec1091d152cbcf5c31d'
};
application action='update' datasources=dsources;

var qry = queryExecute( sql='select * from cb_author', options={ datasource : 'myNewDS' } );

for( var row in qry ) {
    echo( row.firstName & chr( 10 ) );
}
```

So let's break this down real quick.  First we get the current settings of the CLI Lucee context and the list of current databases \(may be null\).  Then we simply add the same datasource definition as above to the struct with the name we wish to use to reference this datasource.  And finally we \`update\` the application with the new struct of datasources. Now we can use this datasource name just we would in a "normal" web application.

## Notes

The internal CLI of CommandBox still runs on Luce 4.5 so make sure you copy the data source definitions from a Lucee 4.5 server, and not a 5.0 server.   Also, you'll note I used encrypted passwords above.  You can also just put the plain text password in.  Just omit the \`encrypted:\` text like so:

```js
username: 'root',
password: 'clear text password'
```



