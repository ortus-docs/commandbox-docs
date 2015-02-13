# Basic Package Data

The following box.json properties provide basic information about what your package is.

## name
**string**

Name of the package. Short, but descriptive.
```bash
package set name="Whiz Bang Module"
package show name
```

## slug
**string**

The unique slug for this package.  Cannot contain spaces or special characters.  Can contain a hyphen.  Use the `forgebox slugcheck` command to see if this slug is already in use.  This is what people will use when installing your package from ForgeBox.
```bash
package set slug="whiz-bang"
package show slug
```

## version
**string**

The semantic version of your package following the pattern `major.minor.patch.build`.  Ex: `2.3.5.0012`
```bash
package set version=1.0.0.0000
package show version
```

## author
**string**

The name of the author of the module as a string.
```bash
package set author="Brad Wood <brad@bradwood.com>"
package show author
```

## shortDescription
**string**

Describes what this package is in a couple sentences.  Save the dissertation for the `description`.
```bash
package set description="Install this module to add a bit of Whiz Bang to your app."
package show description
```

## private
**boolean**

A flag that designates if this package is for public sharing on ForgeBox, or private to you and your company.
```bash
package set private=false
package show private
```




