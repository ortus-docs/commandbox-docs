# Installing Lucee Extensions

If your Task Runner requires a Lucee extension which is not already installed into the Lucee instance that powers the CLI, you will need to install it before you can use it.  You can download the lex file and place it inside the `deploy` folder in the Lucee Server context inside the CLI

```
~/.CommandBox/engine/cfml/cli/lucee-server/deploy
```

The extension will get picked up and installed within 60 seconds if CommandBox is running, or immediately on the next start.

You can also use the `installExtension()` method which is part of the base Task to install any extension available on an update provider.

```javascript
// MySQL JDBC Extension
installExtension( '7E673D15-D87C-41A6-8B5F1956528C605F' )
```

Or specify a version like so:

```javascript
// MySQL JDBC Extension
installExtension( '7E673D15-D87C-41A6-8B5F1956528C605F', '8.0.30' )
```
