# Default Command Parameters

Do you also always type certain parameters every time you run a command, like always typing `--force` after `rm`? Any command parameter can be defaulted at a global level so you don't have to type it every time. These defaults will always be overridden if you actually supply the parameter when running the command.

```text
config set command.defaults.rm.force=true
rm myFile.txt
```

The above example is the same as running `rm myFile.txt --force` since we've defaulted the `force` parameter to always be true when not otherwise specified. If you wanted to override your default, you could do so by actually specifying the parameter from the CLI like this:

```text
rm myFile.txt --NoForce
```

