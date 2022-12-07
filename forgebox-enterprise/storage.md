# Storage

Every FORGEBOX Enterprise includes up to 250GB of binary storage with elastic capabilities.

## Storing Package Binaries on ForgeBox

ForgeBox can store the binaries for your packages in the ForgeBox Cloud. This provides you with an easy way to store multiple versions of your package distributed across the globe.

To utilize ForgeBox Storage, simply set `forgeboxStorage` as the value of your package's `location`.

```bash
CommandBox:my-package> package set location=forgeboxStorage
```

When you publish a package, CommandBox will automatically zip up your package and send it to ForgeBox.
