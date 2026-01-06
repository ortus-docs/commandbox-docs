---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/Bw6M3PI3e5HgLVcKZz0G/embedded-server/install-lucee-core-lex-files
---

# Install Lucee core/lex files

The `install` command will automatically find a local Lucee Server when installing a package that is a Lex file.  There is also now a command called `server lucee-deploy` to help with installing Lex files (Lucee Extensions) and LCO files (Lucee Core).

It will accept an absolute or relative local file path:

```bash
server lucee-deploy myFile.lex
```

or an HTTP URL to download

```bash
server lucee-deploy https://domain.com/path/to/Lucee-core-patch.lco
```

You can also override the name of the CommandBox server to install into, regardless of what working directory the shell is in:

```bash
server lucee-deploy myFile.lex myServer
```

The command will copy the lex or lco file to the deploy folder of the chosen Lucee server following these rules

* If server name is passed as second arg, use that server name (providing it’s a Lucee server)
* Otherwise, if this command is being run inside of a server-related interceptor, capture the name of the server from the intercept data and use that server
* otherwise, look for any Lucee server whose webroot points to the current working directory of the shell and use the first found
* Otherwise, look for a `LUCEE_DEPLOY_DIRECTORY` env var and use this as the deploy directory (can be relative or absolute)

