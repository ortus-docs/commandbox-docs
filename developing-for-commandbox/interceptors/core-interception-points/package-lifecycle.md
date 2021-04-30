# Package Lifecycle

## preInstall

Announced prior to installing a package. If a package has additional dependencies to install, each of them will fire this interception point.

**interceptData**

* `installArgs` - Struct containing the following keys used in installation of the package.
  * `ID` - The ID of the package to install
  * `directory` - Directory to install to.  May be null, if none supplied.
  * `save` - Flag to save box.json dependency
  * `saveDev` - Flag to save box.json dev dependency
  * `production` - Flag to perform a production install
  * `currentWorkingDirectory` - Original working directory that requested installation
  * `verbose` - Flag to print verbose output
  * `force` - Flag to force installation
  * `packagePathRequestingInstallation` - Path to package requesting installing.  This climbs the folders structure for nested dependencies.

## onInstall

Announced while a package is being installed, after the package endpoint and installation directory has been resolved but before the actual installation occurs. This allows you to override things like the installation directory based on package type. Any values updated in the `interceptData` struct will override what the `install` command uses.

**interceptData**

* `installArgs` - Same as `preInstall` above
* `installDirectory` - Directory that the package will be installed in
* `containerBoxJSON` - A struct containing the `box.json` of the page requesting the installation
* `artifactDescriptor` - A struct containing the `box.json` of the package artifcat about to be installed
* `artifactPath` - The folder containing the unzipped artifact, ready to be installed.
* `ignorePatterns` - An array of file globbing patterns to ignore on installation
* `endpointData` - A struct containing the following keys.
  * `endpointName` - The name of the endpoint.  i.e. "forgebox" or "HTTP"
  * `package` - The name of the package. i.e. "cborm" or "coldbox"
  * `ID` - The canonical ID of the endpoint.  i.e. "forgebox:coldbox" or "github:user/repo"
  * `endpoint` - The instance of the endpoint CFC that implements `IEndpoint`.

## postInstall

Announced after an installation is complete. If a package has additional dependencies to install, each of them will fire this interception point. This fires even if an install is skipped due to an existing package that satisfies the dependencies, or if the package is already installed.

**interceptData**

* `installArgs` - Same as `preInstall` above

## preUninstall

Announced before the uninstallation of a package.

**interceptData**

* `uninstallArgs` - Struct containing the following keys used in removal of the package
  * `ID` - ID of the package to uninstall
  * `directory` - The directory to be uninstalled from \(used to find box.json\)
  * `save` - Whether to update box.json
  * `currentWorkingDirectory` - Path to package requesting removal .  This climbs the folders structure for nested dependencies.

## postUninstall

Announced after the uninstallation of a package.

**interceptData**

* `uninstallArgs` - Same as `preUninstall` above

## preInstallAll 

Announced once before all dependencies are installed, no matter how many are actually installed.

**interceptData**

* `installArgs` - Raw parameters passed to the `install` command.

## postInstallAll

Announced once after all dependencies are installed, no matter how many are actually installed.

**interceptData**

* `installArgs` - Raw parameters passed to the `install` command.

## preVersion

Announced before the new version is set in the package.

**interceptData**

* `versionArgs` - A struct containing the following keys:
  * `version` - The new version about to be set
  * `tagVersion` - Boolean that determines whether to tag a Git repo
  * `message` - Commit message to use when tagging Git repo
  * `directory` - The working directory of the package
  * `force` - If true, tag a Git repo even if it isn't clean

## postVersion

Announced after the new version is set in the package but before the Git repo is tagged.

**interceptData**

* `versionArgs` - Same as `preVersion` above.

## onRelease

Announced after a new version is set using the `bump` command and after the Git repo is tagged.

**interceptData**

* `directory` - The working directory of the package
* `version` - The new version about was set

## prePublish

Announced prior to publishing a package to an endpoint

**interceptData**

* `publishArgs` - A struct containing the following keys:
  * `endpointName` - The name of the endpoint being published to
  * `directory` - The directory that the package lives in
* `boxJSON` - A struct containing the defaulted `box.json` properties for the package

## postPublish

Announced after publishing a package to an endpoint

**interceptData**

* `publishArgs` - Same as `prePublish` above.
* `boxJSON` - Same as `prePublish` above.
* **preUnpublish**

Announced prior to unpublishing a package from an endpoint

**interceptData**

* `unpublishArgs` - A struct containing the following keys:
  * `endpointName` - The name of the endpoint being published to
  * `directory` - The directory that the package lives in
  * `version` - The version being unpublished
  * `force` - Boolean to skip the interactive prompt
* `boxJSON` - A struct containing the defaulted `box.json` properties for the package

## postUnpublish

Announced after unpublishing a package from an endpoint

**interceptData**

* `unpublishArgs` - Same as `preUnpublish` above.
* `boxJSON` - Same as `preUnpublish` above.

