# Downloading Files

When you need to download a file from HTTP\(S\) in a Task Runner, you'll want to use our progressable downloader helper.  It has several advantages over a basic HTTP call:

* Automatically uses any configured proxy settings
* Doesn't lock the CLI during download but shows a handy progress bar to the user.
* Can be interrupted with Ctrl-C for very large downloads that the user wants to cancel.

To use the progressable downloader, ask WireBox to inject the following two CFC instances into your Task Runner:

```javascript
property name="progressableDownloader" 	inject="ProgressableDownloader";
property name="progressBar" 			inject="ProgressBar";
```

Then, when you're ready to download, use them like so:

```javascript
var result = progressableDownloader.download(
	'http://site.com/fileToDownload.zip',
	'C:/path/to/fileWeDownloaded.zip',
	// This callback fires every 1024K of downloaded bytes
	function( status ) {
		progressBar.update( argumentCollection = status );
	}
);
```

That will download the file and place it in the local path.  The closure is a callback that updates the progress bar as the file downloads.  It is decoupled this way so you could make your own progress bar if you wanted.

The `result` variable contains the following struct.

```javascript
{
	responseCode,
	responseMessage,
	headers // struct
}
```

An error will be thrown if the status code is less than 200 or greater than 399. The progressable downloader will follow 301 and 302 redirects automatically.  If you want to track this, you can add an additional listener closure which is called for every redirect. 

```javascript
progressableDownloader.download(
	downloadURL,
	localPath,
	function( status ) {
		progressBar.update( argumentCollection = status );
	},
	function( newURL ) {
		print.line( "Redirecting to: '#arguments.newURL#'..." );
	}
);
```

