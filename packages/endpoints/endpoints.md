# Code Endpoints

For CommandBox to be able to install packages for you it needs to connect to package registry where packages are stored so it can download them for installation. CommandBox integrates seamlessly with ForgeBox, our community of ColdFusion (CFML) projects. CommandBox also integrates with HTTP(S), local file/folder, and Git endpoints.

## Supported Endpoints

Here is a list of the package endpoints currently supported by CommandBox.

* **ForgeBox** - Cloud-based packages [(*Read more*)](forgebox.md)
* **HTTP(S)** - Point to a hosted zip file containing a package  [(*Read more*)](https.md)
* **File**  - A local file containing a package [(*Read more*)](file.md)
* **Folder** - A local folder containing a package [(*Read more*)](folder.md)
* **Git** - Any Git repo containing a package [(*Read more*)](git.md)
* **Jar** - A jar file hosted via HTTP that's _not_ contained in a zip file [(*Read more*)](jar-via-http.md)
* **CFLib** - UDFs posted on CFLib.org [(*Read more*)](cflib.md)
* **RIAForge** - Projects posted to RIAForge.org  [(*Read more*)](riaforge.md)

## Examples

```bash
install coldbox@3.8.1
install http://www.site.com/myPackage.zip
install /var/libs/myPackage.zip
install /var/libs/myPackage/
install git://github.com/username/repoName.git#v1.5.6
```