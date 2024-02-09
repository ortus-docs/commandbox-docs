# Printing Tree

You can print hierarchical data as an ASCII tree, like how the `package list` command works by using the `print.tree()` helper.  The `tree()` helper accepts a top level struct where each key will reprent a top level branch in the tree using the name of the key.  If the value of the key is a nested struct, items will be nested below based on the keys in that struct.  For a leaf node (no children), the key can have an empty struct or an empty string as the value.

The tree helper will obey the order of the keys, so use ordered structs if the order of output is important to you.

```javascript
print.tree(  [
	'Ortus Solutions' : [
		'Products' : [
			'Open Source' : {
				'ColdBox MVC' : {},
				'CommandBox CLI' : {},
				'ContentBox CMS' : {},
				'WireBox' : {},
				'TestBox' : {},
				'CacheBox' : {}
			},
			'Commercial' : {
				'ForgeBox Pro' : {},
				'CommandBox Pro' : {},
				'CommandBox Service Manager' : {},
				'TimeBox BMP' : {}
			}
		],
		'Services' : {
			'Consulting' : {
				'Ad-Hoc hours' : {},
				'Hourly Retainer' : {},
				'Custom' : {}
			},
			'Training' : {},
			'Design' : {}
		},
		'Employees' : {
			'Luis' : {},
			'Brad' : {},
			'Gavin' : {},
			'Eric' : {},
			'Jon' : {},
			'Jorge' : {},
			'Edgardo' : {}
		}
	]
] )
```

which outputs the following:

```
└─┬ Ortus Solutions
  ├─┬ Products
  │ ├─┬ Open Source
  │ │ ├── CacheBox
  │ │ ├── WireBox
  │ │ ├── CommandBox CLI
  │ │ ├── ContentBox CMS
  │ │ ├── TestBox
  │ │ └── ColdBox MVC
  │ └─┬ Commercial
  │   ├── ForgeBox Pro
  │   ├── CommandBox Service Manager
  │   ├── TimeBox BMP
  │   └── CommandBox Pro
  ├─┬ Services
  │ ├── Training
  │ ├── Design
  │ └─┬ Consulting
  │   ├── Hourly Retainer
  │   ├── Ad-Hoc hours
  │   └── Custom
  └─┬ Employees
    ├── Brad
    ├── Gavin
    ├── Edgardo
    ├── Jorge
    ├── Eric
    ├── Luis
    └── Jon

```

The `tree()` method also accepts a second argument which is a closure that is called for every item in the tree, returning a string to influence the formatting of that item.  The closure receives

* All parent keys concatenated as a single string
* All parent keys as an array

So, for example, if you were outputting a tree view of file listings, where the top level key was `C:/` and next level was `Windows/` and the leaf node was `foo.txt`, the string version of the key path would be `C:/Windows/foo.txt` as passed to the closure.  You can use the hierarchy to color entire parts of the tree if you wish.  e.g., all items with the prefix `C:/Windows/` are blue, etc.

There is no CLI equivalent to this helper since generating the input data in the needed format would be a little difficult.  &#x20;
