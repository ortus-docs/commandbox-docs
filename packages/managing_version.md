# Managing Package Version

One of the more important pieces of information for a package is the version.  We encourage people to use semantic versioning for their packages.  The spec for this can be read here:

[http://semver.org](http://semver.org)

The spec involves a lot of possible variations, but to simplify, the basics involve having a three-part version number.   

# Versioning
CommandBox is maintained under the [Semantic Versioning](http://semver.org) guidelines as much as possible.  Releases will be numbered with the following format:

```
<major>.<minor>.<patch>.<buildID>
```

And constructed with the following guidelines:

* Breaking backward compatibility bumps the major (and resets the minor and patch)
* New additions without breaking backward compatibility bumps the minor (and resets the patch)
* Bug fixes and misc changes bumps the patch