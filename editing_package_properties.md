# Editing Package Properties

We've seen how any folder can be turned into a package with the `init` command.  Initial properties can be set as named parameters to the `init` command.  

```bash
init name="My Package" version=1.0.0
```

The `init` command can be called more than once on a package and it will keep existing properties and only overwrite the ones you specify.  
Once you've created your box.json, you can edit the file directly, or there are other commands to help manage it programmatically.  

Each of these commands support tab-complete that is dynamic based on what properties are currently in your box.json.

## package show

The `package show` command can be used to out put any part  of the box.json.  You can specifiy any property name from your box.json.  

Outputs package name and keywords
```bash
package show name
package show keywords
```

Nested attributes may be accessed by specifying dot-delimited names or using array notation.
If the accessed property is a complex value, the JSON representation will be displayed

Outputs testbox runner(s)
package show keywords
package show testbox.runner

Outputs the first testbox notify E-mail

package show testbox.notify.emails[1]

## package set

## package clear