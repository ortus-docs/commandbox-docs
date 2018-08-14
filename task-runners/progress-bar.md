# Progress Bar

When writing a Task that needs to iterate some predefined amount of work that might take some time, you can now tap into the Progress Bars in CommandBox easily for your own purposes.

There are two different progress bars in CommandBox.  One specifically for downloading files, and a generic one.

## Progressable Downloader

Here is how you can download a file in a Task Runner and have a progress bar animation that contains all the usual data such as download speed and file size.  The progressable downloader will automatically take any HTTP proxy settings into account.

![](../.gitbook/assets/image%20%281%29.png)

If the remote server doesn't send a content length header \(like S3 cloudfront\) then instead of a progress bar, you'll just see a climbing file size that shows how much as been downloaded so far.  

```javascript
component {
	property name="progressableDownloader"	inject="ProgressableDownloader";
	property name="progressBar" 			inject="ProgressBar";
	
	function run() {
		progressableDownloader.download(
			'http://site.com/file.zip',
			'C:/savedFileHere.zip',
			function( status ) {
				progressBar.update( argumentCollection = status );
			}
		);
	}
}
```

That is all you need.  The **ProgressableDownloader** class takes care of the actual downloading of the file and the **ProgessBar** class deals with drawing the progress bar to the screen.  

## Generic Progress Bar

You can also use a generic progress bar for any purpose.  It is up to you to update the progress bar with the current percentage.  If you are inside of an Interactive Job, the progress bar will automatically show at the bottom of the job output.

![](../.gitbook/assets/image%20%287%29.png)

```javascript
component {
	property name="progressBarGeneric" inject="progressBarGeneric";
	
	function run() {
	    // Draw initial progress bar
		progressBarGeneric.update( percent=0 );
		
		// Update as we progress
		sleep( 1000 );
		progressBarGeneric.update( percent=25, currentCount=250, totalCount=1000 );
		sleep( 1000 );
		progressBarGeneric.update( percent=50, currentCount=500, totalCount=1000 );
		sleep( 1000 );
		progressBarGeneric.update( percent=75, currentCount=750, totalCount=1000 );
		sleep( 1000 );
		// Progress bar automaticaly hides once it reaches 100%
		progressBarGeneric.update( percent=100, currentCount=1000, totalCount=1000 );
	}
}
```

The `currentCount` and `totalCount` parameters to the `update()` method are optional.  They will show in the output only if you provide them.  

### Other Methods

You can remove the progress bar from the screen at any time with `clear()`.

```javascript
progressBarGeneric.clear();
```

You can show the bar again at any percentage simply by calling the `update()` method.  No need to initialize anything.  The progress bar object carries no state.

```javascript
progressBarGeneric.update( percent=27 );
```

