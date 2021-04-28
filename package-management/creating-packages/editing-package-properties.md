# Editing Package Properties

We've seen how any folder can be turned into a package with the `init` command. Initial properties can be set as named parameters to the `init` command.

```bash
init name="My Package" version=1.0.0
```

The `init` command can be called more than once on a package and it will keep existing properties and only overwrite the ones you specify.  
Once you've created your box.json, you can edit the file directly, or there are other commands to help manage it programmatically.

Each of these commands support tab-complete that is dynamic based on what properties are currently in your box.json.

## package show

The `package show` command can be used to out put any part of the box.json. You can specifiy any property name from your box.json.

Outputs package name and keywords

```bash
package show name
package show keywords
```

Nested attributes may be accessed by specifying dot-delimited names or using array notation. If the accessed property is a complex value, the JSON representation will be displayed

Outputs testbox runner\(s\)

```bash
package show testbox.runner
```

Outputs the first testbox notify E-mail

```bash
package show testbox.notify.emails[1]
```

Output the entire box.json

```bash
package show
```

using JmesPath with package show

```bash
package show jq:slug
package show "jq:contributors|split(@,' ')" // grab contributors and split each result by spaces
package show "jq:engines[].type" //get an array of all engine names
package show "jq:{name:name, myprop:'test'}" // filter struct values and add in an additional value => { "myprop":"test", "name":"MyPackageName" }
package show "jq:[name,version]|join('...',@)" //return struct values in an array then join all values together with ... => MyPackageName...2.4
```

## package set

Any property in your box.json can be set from the command line with the `package set` command.

Set package name

```bash
package set name=myPackage
```

Nested attributes may be set by specifying dot-delimited names or using array notation. If the set value is JSON, it will be stored as a complex value in the box.json.

Set the repository type.

```bash
package set repository.type=Git
```

Set first testbox notify E-mail.

```bash
package set testbox.notify.email[1]="brad@bradwood.com"
```

Set multiple params at once by passing as many named parameters as you like.

```bash
package set name=myPackage version="1.0.0.000" author="Brad Wood" slug="foo"
```

Set a complex value as JSON.

```bash
package set testbox.notify.emails="[ 'test@test.com', 'me@example.com' ]"
```

### Appending Complex Values

Objects and arrays can be appended to using the `append` parameter. This only works if the property and incoming value are both of the same complex type.

Add an additional contributor to the existing `contributors` array.

```bash
package set contributors="[ 'brad@coldbox.org' ]" --append
```

Add an additional dependency to the existing `dependencies` object.

```bash
package set dependencies="{'cbcommons':'1.0.0'}"  --append
```

## package clear

If you need to remove a property entirely from your box.json, use the `package clear` command. It also works on nested properties using "dot" or array notation.

Remove the package description entirely.

```bash
package clear description
```

