# Code Endpoints

For CommandBox to be able to install packages for you it needs to connect to package registry where packages are stored so it can download them for installation. CommandBox integrates seamlessly with ForgeBox, our community of ColdFusion (CFML) projects. CommandBox also integrates with HTTP(S), local file/folder, and Git endpoints.

## Supported Endpoints

Here is a list of the package endpoints currently supported by CommandBox.

* [**ForgeBox**](http://ortus.gitbooks.io/commandbox-documentation/content/packages/endpoints/forgebox.html) - Cloud-based packages
* [**HTTP(S)**](http://ortus.gitbooks.io/commandbox-documentation/content/packages/endpoints/https.html) - Point to a hosted zip file containing a package 
* [**File**](http://ortus.gitbooks.io/commandbox-documentation/content/packages/endpoints/file.html)  - A local file containing a package
* [**Folder**](http://ortus.gitbooks.io/commandbox-documentation/content/packages/endpoints/folder.html) - A local folder containing a package
* [**Git**](http://ortus.gitbooks.io/commandbox-documentation/content/packages/endpoints/git.html) - Any Git repo containing a package

## Examples

```bash
install coldbox@3.8.1
install http://www.site.com/myPackage.zip
install /var/libs/myPackage.zip
install /var/libs/myPackage/
install git://github.com/username/repoName.git#v1.5.6
```