# Git Endpoint

Entire Git repos that represent a package can be installed via the Git endpoint.  This can be a public Git server like GitHub or Bitbucket, or a private Git repo behind your firewall.  

Make sure the root of your Git repo has a `box.json` inside of it so CommandBox can tell the version and name of the package.  If there is no `box.json`, the name of the repo will be used as the package name.

## Installation

To install a package from a Git repo, use the URL like so:

```bash
install git://github.com/username/repoName.git
```

You can target a specific `branch`, `tag`, or `commit` by adding a "commit-ish" to the end of the URL.

```bash
install git://site.com/user/repo.git#development
install git://site.com/user/repo.git#v1.2.3
install git://site.com/user/repo.git#09d302b4fffa0b988d1edd8ea747dc0c0f2883ea
```

## GitHub shortcut

If the repo you wish to install is located on Github.com, you can use this shortcut to specifying the package.


```bash
install username/repoName
```

## In box.json

You can specify packages from folder endpoints as dependencies in your `box.json` in this format.  Remember, JSON requires that backslashes be escaped.

```javascript
{
    "dependencies" : {
        "myPackage" : "git://github.com/username/repoName.git"
    }
}

```

## Authentication

Git repos that allow anonymous pulls do not require any additional configuration for authentication.  CommandBox's Git endpoint supports SSH authentication via public/private keys by using the `git+ssh://` protocol. 

```bash
install git+ssh://site.com:user/repo.git#v1.2.3
```

The `git+ssh` endpoint will look for a private SSH key in your `~/.ssh` directory named `id_rsa`, `id_dsa`, or `identity`.  If you are using a multi-key setup with a `~/ssh/config` file, it will be read, and the appropriate key will be used for the host.  The matching public key needs to be registered in the Git server. 

Password authentication is not supported yet for HTTP, HTTPS, or SSH Git protocols.  