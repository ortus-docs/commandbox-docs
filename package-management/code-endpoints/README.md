# Code Endpoints

For CommandBox to be able to install packages for you it needs to connect to package registry where packages are stored so it can download them for installation. CommandBox integrates seamlessly with ForgeBox, our community of ColdFusion \(CFML\) projects. CommandBox also integrates with HTTP\(S\), local file/folder, and Git endpoints.

## Supported Endpoints

Here is a list of the package endpoints currently supported by CommandBox.

* **ForgeBox** - Cloud-based packages [\(_Read more_\)](forgebox.md)
* **HTTP\(S\)** - Point to a hosted zip file containing a package  [\(_Read more_\)](http-s.md)
* **File**  - A local file containing a package [\(_Read more_\)](file.md)
* **Folder** - A local folder containing a package [\(_Read more_\)](folder.md)
* **Git** - Any Git repo containing a package [\(_Read more_\)](git.md)
* **Jar** - A jar file hosted via HTTP that's _not_ contained in a zip file [\(_Read more_\)](jar-via-http.md)
* **S3** - A package zip stored in a private S3 bucket [\(_Read more_\)](s3.md)
* **CFLib** - UDFs posted on CFLib.org [\(_Read more_\)](cflib.md)
* **RIAForge** - Projects posted to RIAForge.org  [\(_Read more_\)](riaforge.md)
* **Gist** - A package hosted as a Gist from gist.github.com  [\(_Read more_\)](https://github.com/ortus-docs/commandbox-docs/tree/6084bc90d6e41d835d8064b6c3ab1cb14ca9d618/package-management/code-endpoints/gist.md)

## Examples

```bash
install coldbox@3.8.1
install http://www.site.com/myPackage.zip
install /var/libs/myPackage.zip
install /var/libs/myPackage/
install git://github.com/username/repoName.git#v1.5.6
```

