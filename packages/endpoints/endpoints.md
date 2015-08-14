# Code Endpoints

For CommandBox to be able to install packages for you it needs to connect to package registry where packages are stored so it can download them for installation. CommandBox integrates seamlessly with ForgeBox, our community of ColdFusion (CFML) projects. CommandBox also integrates with HTTP(S), local file/folder, and Git endpoints.

## Supported Endpoints

Here is a list of the package endpoints currently supported by CommandBox.

* **ForgeBox** - Cloud-based packages [(*Read more*)](http://ortus.gitb ooks.io/commandbox-documentation/content/packages/endpoints/forgebox.html)
* **HTTP(S)** - Point to a hosted zip file containing a package  [(*Read more*)](http://ortus.gitbooks.io/commandbox-documentation/content/packages/endpoints/https.html)
* **File**  - A local file containing a package [(*Read more*)](http://ortus.gitbooks.io/commandbox-documentation/content/packages/endpoints/file.html)
* **Folder** - A local folder containing a package [(*Read more*)](http://ortus.gitbooks.io/commandbox-documentation/content/packages/endpoints/folder.html)
* **Git** - Any Git repo containing a package [(*Read more*)](http://ortus.gitbooks.io/commandbox-documentation/content/packages/endpoints/git.html)

## Examples

```bash
install coldbox@3.8.1
install http://www.site.com/myPackage.zip
install /var/libs/myPackage.zip
install /var/libs/myPackage/
install git://github.com/username/repoName.git#v1.5.6
```