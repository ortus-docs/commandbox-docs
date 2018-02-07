# Private ForgeBox Packages

ForgeBox allows you to publish packages that only you can see and install.  You'll be able to view your private package from the CLI, in the ForgeBox.io search, and in your ForgeBox.io profile, but these packages will now show up for any other users.

## Publishing private packages to ForgeBox

To create a private package, set the `private` property to `true` in your `box.json` prior to publishing.

```
package init
package set private=true
package set slug=my-slug@forgeBoxUser
etc...
publish
```

## Installing private packages from ForgeBox

Replace `forgeBoxUser` with your actual ForgeBox username.  When you install the package, you'll need to use the full slug like so:

```
install my-slug@forgeBoxUser
```

You can install specific versions or version ranges as you would expect:

```
install my-slug@forgeBoxUser@1.0.0
install my-slug@forgeBoxUser@be
```

> Private packages will be a paid feature for ForgeBox Pro subscribers, though the feature is currently available to all users for free.



