# Publishing Lucee Extensions to ForgeBox

ForgeBox.io can be used as a general purpose community extension provider for Lucee 5 compatible extensions. All you need to do is publish your extensions to Forgebox just like you would with any other package, and then anyone who adds [https://www.forgebox.io](https://www.forgebox.io) to their Lucee server as an extension provider will be able to see you extension and install any version of it that they choose. This allows anyone to share their extensions with the world without the need to create, host, and maintain their own publish extension provider.

## Requirements

In order to publish you extension (or any other package for that matter) to ForgeBox you just need three things

1. A forgebox account (use `forgebox register` if you don’t have one)
2. A box.json with the correct metadata in your package.
3. Run the `publish` command from that folder.

A new package will be created if it doesn’t exist, otherwise it will be updated. A new version will also be created if it doesn’t exist matching the version in your `box.json` or it will be updated. Review our generic docs on how to creating and publishing packages here: [https://ortus.gitbooks.io/commandbox-documentation/content/packages/creating\_packages/creating\_packages.html](https://ortus.gitbooks.io/commandbox-documentation/content/packages/creating\_packages/creating\_packages.html)

## Package Metadata

In your `box.json`, you’ll want to minimally have the following properties set:

* The `slug` property in your `box.json` to be the unique GUID of your extension from your manifest file. Lucee's docs state this needs to be a UUID.  It needs to match what’s in your manifest or updates won’t work.
* The `version` needs to be the current version of your package that you want to publish. To add a new version, you’ll just update the json and re-run the `publish` command. One thing to watch out for is that Lucee likes to use the `x.y.z.q` version format which does not quite match the npm-style `x.y.z-prerelease+build` format of ForgeBox. I usually stick with just three digits `x.y.z` so it’s compatible across the board.
* You want the `type` property in your json to be `lucee-extensions`
* Set your publicly-acessable thumbnail URL in a `thumbnail` property in the json
* The actual download URL of the lex file needs to be set in the `location` property in the json. Please note Lucee has some bugs where it doesn’t like servers that don’t set the right content type that it expects. Someone else I was helping had to rename it to a zip file on GitHub so get Lucee to accept it. (A lex file is just a zip file)  Perhaps go give a vote on [this ticket](https://luceeserver.atlassian.net/browse/LDEV-1723).
* Fill out any other info like `name` (human readable), `author`, etc

## Package Description

The `publish` command will pick up any `readme.md` file in your current directory where you run the `publish` command and will put it on ForgeBox as the description. This is very handy so make sure you have a good readme so your package home looks good. If you want to update it, simply edit the readme file and re-run the `publish` command at any time.

## Sample box.json

Here is the `box.json` file used for the [Ortus Couchbase Extension](https://www.forgebox.io/view/14B0C2A7-5CFC-4606-9167C93959A8B82C).

```javascript
{
    "name":"Ortus Couchbase Cache",
    "version":"3.0.0",
    "author":"Brad Wood",
    "location":"https://s3.amazonaws.com/downloads.ortussolutions.com/ortussolutions/couchbase-extension/couchbase-cache-3.0.0.lex",
    "slug":"14B0C2A7-5CFC-4606-9167C93959A8B82C",
    "description":"A Lucee caching extension allow you to connect to a Couchbase Cluster for caching, session, and client storage",
    "type":"lucee-extensions",
    "thumbnail":"https://s3.amazonaws.com/downloads.ortussolutions.com/ortussolutions/couchbase-extension/couchbase-cache-logo.png"
}
```

## Advanced properties

Lucee extensions have additional metadata that can be provided in their manifest that an extension provider reports. These additional properties can be specified in your `box.json` and the ForgeBox Lucee Extension provider will pick them up and report them. Please refer to the [Lucee docs](http://docs.lucee.org/guides/lucee-5/extensions.html) on what each of these do.

* `lucee-extension-category`
* `lucee-extension-releasetype`
* `lucee-extension-minloaderversion`
* `lucee-extension-mincoreversion`
* `lucee-extension-price`
* `lucee-extension-currency`
* `lucee-extension-disablefull`
* `lucee-extension-trial`
* `lucee-extension-promotionlevel`
* `lucee-extension-promotiontext`
