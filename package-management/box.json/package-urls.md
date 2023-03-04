# Package URLs

There are a number of URLs associated with your package that you can specify with these properties.

## location

**string**

The location of where to download this package. Needs to point to a zip file containing the package.

```bash
package set location="https://github.com/ColdBox/coldbox-platform/archive/master.zip"
package show location
```

## homepage

**string**

The homepage for your product. It's fine if this is just the GitHub page. A nice readme can make for a good homepage.

```bash
package set homepage="http://www.coldbox.org"
package show homepage
```

## documentation

**string**

This is where users can go to find documentation on how to use your package. Can be a wiki, HTML docs, etc.

```bash
package set documentation="http://wiki.coldbox.org"
package show documentation
```

## repository

**object**

This describes the repository used for source control with this package. Object needs a `type` and `URL` key. Examples of `type` would be Git, SVN, Mercurial, etc.

```javascript
"repository" : { 
    "type" : "Git",
    "URL" : "https://github.com/ColdBox/coldbox-platform.git"
}
```

```bash
package set repository="{ type : 'Git', URL : 'https://github.com/ColdBox/coldbox-platform.git' }"
package show repository
```

## bugs

**string**

This is the URL where developers can go to report bugs in your package. We know you NEVER write bugs in your software, but it's just a formality so humor us :)

```bash
package set bugs="https://ortussolutions.atlassian.net"
package show bugs
```

## projectURL

**string**

This is the URL you visit to browse to this actual project code once it is running.

```bash
package set projectURL="http://localhost"
package show projectURL
```
