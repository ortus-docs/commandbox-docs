# Artifacts

CommandBox automatically keeps a local copy of every package it downloads called an artifact. These are stored in the `~/.CommandBox/artifacts/` folder. The next time you install the same package, the file in your local artifacts cache will be used to prevent another download.

When installing a package, CommandBox will check its local artifacts first and use the zip file found there first.  If nothing is found, the package will be downloaded from the ForgeBox download URL and then cached in the artifacts directory for next time.  

Artifacts are organized by folders named after the package slug, and then folders inside named after the version.  You can get a list of all your artifacts with the `artifacts list` command.

```bash
CommandBox> artifacts list
Found 5 artifact(s) (/.CommandBox/artifacts)
cbcompat - 1 version(s)
  *1.0.1.0
cbstorages - 1 version(s)
  *1.0.0
coldbox-be - 1 version(s)
  *4.0.0
cbjavaloader - 1 version(s)
  *1.0.0
cbfeeds - 1 version(s)
  *1.0.0
```

You can clear individual artifacts by slug and version with the `artifacts remove` command.  Or just wipe all of them with `artifacts clean`.

> **info** The artifact cache is only used by the ForgeBox endpoint.  It is the only remote endpoint capable of telling CommandBox what the latest version of a package is without downloading it again.

## Snapshot versions

CommandBox will cache snapshot package versions in the artifacts cache, but it will still download them every time.  A snapshot version is one that has a preReleaseID of `snapshot`.  
```
install myPackage@1.2.3-snapshot
```
The reason we store it in the artifacts directory is because it might still get used if you attempt to do an offline install.  CommandBox will check the artifacts folder as a last-ditch attempt if ForgeBox is down. 

## Custom Artifacts Directory

You can control where your artifact cache is stored with the `artifactsDirectory` config setting. This can be useful to keep your primary drive from filling up, or to point your files to a shared network drive that your coworkers can share.

```
config set artifactsDirectory=/path/to/artifacts
```