# Package URLs

There are a number of URLs associated with your package that you can specify with these properties.

## location

**string**

The location this package's code is stored.  Can be a URL, Git/SVN enpoint, etc.

```bash
package set
package show
```

## homepage

**string**

The homepage for your product.  It's fine if this is just the GitHub page.  A nice readme can make for a good homepage.

```bash
package set
package show
```

## documentation

**string**

This is where users can go to find documentation on how to use your package.  Can be a wiki, HTML docs, etc.

```bash
package set
package show
```

## repository

**object**

This describes the repository used for source control with this package.  Object needs a `type` and `URL` key.  Examples of `type` would be Git, SVN, Mercurial, etc.  

```bash
package set
package show
```

## bugs

**string**

This is the URL where developers can go to report bugs in your package.  We know you NEVER write bugs in your software, but it's just a formality so humor us :)

```bash
package set
package show
```

## projectURL

**string**

This is the URL you visit to browse to this code once it is running.  

```bash
package set
package show
```
