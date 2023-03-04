# Debug Installation

There is a helpful command called `forgebox version-debug` which will show you what version of a package will be installed without actually installing it.  It can also be useful to test a semver range and see what packages it matches.

So, for example, if you wanted to see what ColdBox version would be installed if you were to run

```bash
install coldbox@6.x
```

then you can instead run this

```bash
forgebox version-debug coldbox@6.x
```

&#x20;You will see output similar to this:

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

In order to filter the output to only show versions which matched your semantic version, use the `--showMatchesOnly`  flag

```bash
forgebox version-debug coldbox@6.x --showMatchesOnly
```

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
