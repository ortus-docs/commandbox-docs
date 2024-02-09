# Git

Entire Git repos that represent a package can be installed via the Git endpoint. This can be a public Git server like GitHub or Bitbucket, or a private Git repo behind your firewall.

Make sure the root of your Git repo has a `box.json` inside of it so CommandBox can tell the version and name of the package. If there is no `box.json`, the name of the repo will be used as the package name.

## Installation

To install a package from a Git repo, use the URL like so:

```bash
install git://github.com/username/repoName.git
install git+https://github.com/username/repoName.git
install git+ssh://git@github.com:username/repoName.git
```

You can target a specific `branch`, `tag`, or `commit` by adding a "commit-ish" to the end of the URL.

```bash
install git://site.com/user/repo.git#development
install git://site.com/user/repo.git#v1.2.3
install git://site.com/user/repo.git#09d302b4fffa0b988d1edd8ea747dc0c0f2883ea
```

If you don't specify a commit-ish, the Girt endpoint will assume you want the `master` branch.   If there is no `master` branch,  we will attempt to checkout the `main` branch.

## GitHub shortcut

If the repo you wish to install is located on Github.com, you can use this shortcut to specifying the package.

```bash
install username/repoName
```

## In box.json

You can specify packages from folder endpoints as dependencies in your `box.json` in this format. Remember, JSON requires that backslashes be escaped.

```javascript
{
    "dependencies" : {
        "myPackage" : "git://github.com/username/repoName.git"
    }
}
```

## Authentication

Git repos that allow anonymous pulls do not require any additional configuration for authentication. CommandBox's Git endpoint supports SSH authentication via public/private keys by using the `git+ssh://` protocol.

```bash
install git+ssh://site.com:user/repo.git#v1.2.3
```

Some Git endpoints (like private Github repos) need a user before the site name in the url string like below:

```bash
install git+ssh://git@github.com:user/repo.git
```

> **Info** Note the git+ssh URL is a little different than a HTTP(S) URL. There is a colon (`:`) after the host instead of a forward slash (`/`).

The `git+ssh` endpoint will look for a private SSH key in your `~/.ssh` directory named `id_rsa`, `id_dsa`, or `identity`. If you are using a multi-key setup with a `~/.ssh/config` file, it will be read, and the appropriate key will be used for the host. The matching public key needs to be registered in the Git server.

> **Info** If you are deploying to a server and you have not previously logged into the Git server from the new machine you will need to make sure the Git server is added to your `known_hosts` file. The quickest way to do this is to use `git clone git@github.com/user/repo.git` from the terminal OR add the line from your local machine to the server.

### Note:

If you receive an invalid private key exception (**Error cloning git+ssh repository and com.jcraft.jsch.JSchException: Auth fail**), check your version of SSH. OpenSSH (7.8 and newer) generates keys in the new OpenSSH format, which starts with **-----BEGIN OPENSSH PRIVATE KEY-----**. JGit does not support this key format. Generate your key in the classic format using the following:

```bash
ssh-keygen -t rsa -m PEM
```

## Password authentication

You can authenticate to a Git repo over HTTP using a username/password combination or a personal access token. The format looks like this:

```bash
install git+https://username@domain.com/user/repo.git
or
install git+https://username:password@domain.com/user/repo.git
```

### Github

Github personal access tokens can be specified as either

```bash
install git+https://username:keyhere@github.com/user/repo.git
```

or just the personal access token like

```bash
install git+https://keyhere@github.com/user/repo.git
```

and they both appear to work the same. It appears that a private Github repo requires the “**repo**” scope selected for the personal access token.

### GitLab <a href="#gitlab" id="gitlab"></a>

GitLab seems to want a username, but it doesn’t seem to matter what the username is.

```bash
install git+https://whateverYouWantHere:keyhere@gitlab.com/group/repo.git
```

### Env Vars <a href="#env-vars" id="env-vars"></a>

You can use environment variables from the CLI or in your **box.json** to protect sensitive information like passwords and keys.

```bash
set token=myToken
install git+https://user:${token}@gitlab.com/group/repo.git
```

## NetRC

We also support the NetRC file format. Just create a files in your user's home directory called `~/.netrc` or `~/_netrc` with the following format:

```
machine github.com
login myUser
password mypass
```

CommandBox will find this file, match the hostname to the Git repo being cloned, and use the username and password as specified.

### Note:

We do not support any of these username/password options over HTTP as it just seems unwise. Please use HTTPS.

## Known Behaviors

Some users of Circle-CI have reported the following error when trying to clone a Git repo over HTTPS.

```
Error cloning github repositoryorg.eclipse.jgit.api.errors.TransportException: ssh://git@github.com/ColdBox/coldbox-platform.git: Auth fail 
org.eclipse.jgit.errors.TransportException: ssh://git@github.com/ColdBox/coldbox-platform.git: Auth fail 
com.jcraft.jsch.JSchException: Auth fail 
```

It is changing the https github url to ssh in the call. You can remove the config setting like so:

```bash
git config --global --unset "url.ssh://git@github.com.insteadof"
```
