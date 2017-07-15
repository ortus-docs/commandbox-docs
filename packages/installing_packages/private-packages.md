# Private ForgeBox Packages

ForgeBox allows you to publish packages that only you can see and install.  You'll be able to view your private package from the CLI, in the ForgeBox.io search, and in your ForgeBox.io profile, but these packages will now show up for any other users.  

## Publishing private packages to ForgeBox

To create a private package, set the `private` property to `true` in your `box.json` prior to publishing.

```
package init
package set private=true
package set slug=@forgeBoxUser/my-slug
etc...
publish
```

## Installing private packages from ForgeBox

Replace `forgeBoxUser` with your actual ForgeBox username.  When you install the package, you'll need to use the full slug like so:

```
install @forgeBoxUser/my-slug
```

