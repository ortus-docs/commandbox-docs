# What's New in 5.9.0

### Java 17 Support

Java 17 was a breaking change for Java users as it now blocks illegal reflective access.  Both Adobe and Lucee have been slow to address this.  Lucee 5.3.10 mostly seems to run on Java 17, but only the public beta of ColdFusion 2023 (Fortuna) supports Java 17.  You may want to start testing Java 17 out, so the CommandBox CLI and its servers seem to have basic support for running on Java 17 now.  Note, you may need to add additional JVM args to any servers based on the specific Java libraries you use.

### Library Updates

We've bumped library versions such as Redhat Undertow to stay current with recent CVE fixes. &#x20;

### Override package install paths

If you have a project and want all packages of a certain type to use a different-than-normal default install location, you can override each package type just for that project.  Create an **installPathConventions** key in the containing package's box.json which is an object containing keys for each package type you wish to override package install paths for.

```
{
  "installPathConventions" : {
    "modules" : "../lib/modules",
    "mvc" : "../lib/framework",
    "testing" : "they-will-never-find-it-here-lol/"
  }
}
```

Read more: [https://commandbox.ortusbooks.com/package-management/installing-packages/installation-path](https://commandbox.ortusbooks.com/package-management/installing-packages/installation-path)

### ls --tree

You can get recursive file listings now in a tree view by using the --tree flag

```
ls --tree
```

### Tree Print Helper

In task runners and custom commands, you can now tap into the same tree printer that the "package list" and "ls --tree" use to output your own ASCII trees.

```
print.tree(  [
	'Ortus Solutions' : [
		'Products' : [
			'Open Source' : {
				'ColdBox MVC' : {},
				'CommandBox CLI' : {},
				'ContentBox CMS' : {}
			},
			'Commercial' : {
				'ForgeBox Pro' : {},
				'CommandBox Pro' : {},
				'TimeBox BMP' : {}
			}
		],
		'Services' : {
			'Consulting' : {},
			'Training' : {},
			'Design' : {}
		}
	]
] )
```

which outputs

```
└─┬ Ortus Solutions
  ├─┬ Products
  │ ├─┬ Open Source
  │ │ ├── CommandBox CLI
  │ │ ├── ContentBox CMS
  │ │ └── ColdBox MVC
  │ └─┬ Commercial
  │   ├── ForgeBox Pro
  │   ├── TimeBox BMP
  │   └── CommandBox Pro
  └─┬ Services
    ├── Training
    ├── Design
    └── Consulting
```

Read More: [https://commandbox.ortusbooks.com/task-runners/printing-tree](https://commandbox.ortusbooks.com/task-runners/printing-tree)

### Column Print Helper

There is also a print helper method for task runners and custom commands as well as a CLI command you can use that accepts an array or list of simple values and prints them in a column format based on the widest value and the available terminal width.

```
ls --simple | printColumns
```

Read More: [https://commandbox.ortusbooks.com/task-runners/task-output/printing-columns](https://commandbox.ortusbooks.com/task-runners/task-output/printing-columns)

### unansi Command

This command will accepted piped text and strip any ANSI formatting from it.  Especially useful if piping the text to a native OS binary which doesn't handle formatting well.

```
package list | unansi
```

### clipboard Command

We've introduced a new command for piping text onto your native operating system's clipboard. &#x20;

```
echo "Hello World!" | clipboard
Copied to clipboard!
```

### Release Notes

Here's the full list of all the changes in CommandBox 5.9.0.
