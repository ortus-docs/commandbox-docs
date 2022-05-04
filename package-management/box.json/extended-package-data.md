# Extended Package Data

These properties go a little farther in describing your package and what it is.

## description

**string**

The full description of your package. Feel free to use line breaks, HTML, or football analogies.

```bash
package set description="This is a \n cool module \n please install it."
package show description
```

## instructions

**string**

Tell the user how to install and use your package.

```bash
package set instructions="1) Unwrap packaging \n 2) Apply liberally to affected site \n 3) profit!"
package show instructions
```

## changelog

**string**

A list of the changes this package has gone through.

```bash
package set changelog="1.0 initial version \n 1.1 Bug fixes"
package show changelog
```

## keywords

**array**

List words that describe your package as an array of keywords.

```bash
package set keywords="[ 'cool', 'amazing', 'whiz', 'bang', 'cheese whiz' ]" --append
package show keywords
```

## license

**array of objects**

Let the world know what license your package is released under. This property is an array of objects where each object represents a single license. Your package can have more than once license.

The license object will have a `type` and `URL` key. Examples of license types are MIT, GPL, Apache 2.0, etc. A valid list of licenses might look like this:

```javascript
"license" : [
        { "type" : "MIT", "URL" : "http://opensource.org/licenses/MIT" },
        { "type" : "GPL-3.0", "URL" : "http://opensource.org/licenses/GPL-3.0" }

    ]
```

```bash
package set license="[ { type : 'MIT', URL: 'http://opensource.org/licenses/MIT' } ]" --append
package show license
```

## contributors

**array**

Give a shout-out here to everyone who helped with your package. This is an array, and you can put strings it containing names and/or E-mail addresses, or you a objects to the array containing keys such as `name` and `email`.

```javascript
"contributors" : [
        "Mickey Mouse",
        "Minny Mouse <minny@disney.com>",
        { "name" : "Daffy Duck", "email" : "daffy@disney.com" }

    ]
```

```bash
package set contributors="[ 'Goofy' ]" --append
package show contributors
```
