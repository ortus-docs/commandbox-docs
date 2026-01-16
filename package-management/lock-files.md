# Lock Files

As of CommandBox 6.3.0, you can opt-in to lock file support with CommandBox.

Lock files give you predictable installs by recording the exact version of each dependency installed when created or updated.  Then, when installing in a CI build or on another developer's machine, the versions in the lock file are used.  This is especially helpful in CI builds as it helps to make your builds idempotent.

Using lock files does come with a few changes to the developer workflow as updating packages needs to be done through the `update` method or by changing the version range to require a new version of the package.  In practice, most of these changes are minimal when compared to the benefits of lock files.

### Generating a Lock File

To generate a lock file, add the `lock` flag to your install command.

```
install --lock
```

CommandBox will create a `box-lock.json` file in the root of your repository.  This file is utilized solely by the lock feature of CommandBox and does not need to be manually edited.

You only need to generate the lock file once per repository.  You do not need to include the `--lock` flag after generating the lock file.  The existence of the lock file will enable lock file support inside CommandBox.

### Committing the Lock File to Version Control

While the `box-lock.json` file does not need to be manually edited, it should be tracked in version control.  This allows you to see exactly when locked versions were changed to help you debug issues, as well as giving access to locked versions to CI pipelines.

### Installing Packages with an Existing Lock File

If an existing lock file is detected, CommandBox will use it when installing packages.  There are a few caveats and rules to how it will interact with lock files.

#### Only ForgeBox endpoint packages are locked.

Packages installed via other endpoints will be ignored.  Note that this only applies to the endpoint, not the stored location of the package, i.e. you do not need to utilize ForgeBox Storage.&#x20;

#### If a package is not in the lock file, the latest matching semantic version will be installed from ForgeBox and then recorded in the lock file.

This applies to both using the `install packageName` command and adding a package into your `box.json` file and running `install`.

#### If a package is in the lock file and the locked version **satisfies** the semantic version range in your `box.json` file, then the locked version will be used.

This applies to both using the `install packageName` command and adding a package into your `box.json` file and running `install`.

For example, if you have `coldbox` in your `box.json` with a semantic version range of `^8.0.0` and it is also in your lock file with a locked version of `8.0.1`, then running `install` will always install `coldbox@8.0.1`.

If you run `install coldbox` manually, CommandBox first looks up if you have the package in your `box.json` file.  Since you do, it uses the range in your `box.json` file — `^8.0.0`.  That means that no new version would be installed since `8.0.1` satisfies `^8.0.0`.

#### If a package is in the lock file and the locked version **does not satisfy** the semantic version range in your `box.json` file, then the latest matching semantic version will be installed from ForgeBox and then recorded in the lock file.

This applies to both using the `install packageName` command and adding a package into your `box.json` file and running `install`.

For example, if you have `coldbox` in your `box.json` with a semantic version range of `^8.1.0` and it is also in your lock file with a locked version of `8.0.1`, then running `install` will always install the latest version of `coldbox` that is at least version `8.1.0` up to and not including version `9.0.0`.

If you run `install coldbox@^8.1.0` manually, CommandBox will first replace the semantic version range in your `box.json` file.  Then it uses the new range in when checking the lock file. Since `8.0.1` no longer satisfies the semantic version range, CommandBox will install the latest matching version from ForgeBox and update the lock file with the new version.

### Updating Packages

With a lock file detected, the `update` command — whether it is run for all packages, a single package, or a list of packages — will find the latest matching semantic version from ForgeBox and update the lock file with the new version.

This is the primary way to update the versions of locked packages.  It requires specific developer action and intent and records the result in a version-controlled file to track.

