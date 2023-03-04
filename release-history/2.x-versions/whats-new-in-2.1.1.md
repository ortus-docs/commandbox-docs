# What's New in 2.1.1

This fixes a bug in the "update" and "outdated" commands that caused them to error after you had installed packages from an endpoint other than ForgeBox.  Note, packages installed from HTTP(S) and Git endpoints will always show as outdated and will always update since those endpoints don't provide a way to know what version they're hosting without downloading the entire package anyway.

We also included a small enhancement to the Git endpoint to allow for authentication via public/private SSH keys.  As long as you have a public key configured on your Git server and the private key is stored in \~/.ssh/ using a standard name, SSH-based clones should automatically authenticate.  Please see [the docs](http://commandbox.ortusbooks.com/content/packages/endpoints/git.html) for more info.

As always, the [CommandBox Getting Started Guide](http://commandbox.ortusbooks.com/content/getting\_started\_guide.html) is located here:

[http://commandbox.ortusbooks.com/content/getting\_started\_guide.html](http://commandbox.ortusbooks.com/content/getting\_started\_guide.html)

## Release Notes

### Bug

* \[[COMMANDBOX-259](https://ortussolutions.atlassian.net/browse/COMMANDBOX-259)] - update command erroring

### New Feature

* \[[COMMANDBOX-263](https://ortussolutions.atlassian.net/browse/COMMANDBOX-263)] - Git SSH endpoint private key support

### Improvement

* \[[COMMANDBOX-260](https://ortussolutions.atlassian.net/browse/COMMANDBOX-260)] - Standardize parameter names for install command
