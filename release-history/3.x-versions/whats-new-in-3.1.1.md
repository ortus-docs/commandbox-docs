# What's New in 3.1.1

### Multi-Server 

Now CommandBox will not only start up Lucee 4 servers with a single command, but you can start up Adobe ColdFusion, Railo, and even Luce 5 servers all at the same time.  Now it's easier than ever to test your code across multiple platforms.  CommandBox's embedded server makes for a fast and easy development machine too regardless of what CF engine you need.

```bash
# Start the latest stable Railo engine
CommandBox> start cfengine=railo

# Start a specific engine and version
CommandBox> start cfengine=adobe@10.0.12

# Start any Java WAR
CommandBox> start WARPath=/var/www/myApp.war
```

### ForgeBox 2.0 API

We'v released a brand new [ForgeBox.io](https://www.forgebox.io/) site with a new UI, fresh features, and a shiny new API.  CommandBox 3.1.1 is now powered by the new ForgeBox site and API which includes features like having more than one version for a package.  

![](https://www.ortussolutions.com/__media/forgebox2.0.png)

### Semantic Versioning support

When you install packages from [ForgeBox](https://www.forgebox.io/), you can use fancy semver ranges to specify the versions of a package you're willing to install.  CommandBox will automatically grab the latest version that satisfies your version range.  This also applies to the "update" command which makes keeping your projects' dependencies up-to-date even easier.

```bash
# A specific version
CommandBox> install foo@1.2.3

# Any version with a major number of 4 (4.1, 4.2, 4.9, etc)
CommandBox> install foo@4.x

# Any version greater than 1.5.0
CommandBox> install foo@>1.5.0

# Any version greater than 5.2 but less than or equal to 6.3.4
CommandBox> install "foo@>5.2 <=6.3.4"
```

### Create user from CLI

Another feature of the new [ForgeBox](https://www.forgebox.io/) site is the ability to create a new ForgeBox user right from the CLI.  After creation, you'll be logged in with your ForgeBox API Key which let's you update your packages.

```bash
CommandBox> forgebox register
```

### Publish packages from the CLI

You no longer need to visit the ForgeBox web site to publish new or updated packages to ForgeBox.  This is all available from the CLI once you've logged in.  This  means you can even automate the process of publishing to cut down on the number of manual steps it takes you to update your projects and share those changes with the community.

```bash
CommandBox> forgebox publish
```

### Interceptor-based CLI scripts

You can now run commands of your choosing automatically when certain events in the CLI happen \(like publishing a package, or starting a server\).  You can also create ad-hoc collections of commands to run whenever you want to help automate things like building your projects or publishing to ForgeBox.

```javascript
{
  "name" : "My Package",
  "slug" : "my-package",
  "version" : "1.0.0",
  "scripts" : {
   "postVersion" : "package set location='gitUser/gitRepo#`package version`'"
   "postPublish" : "!git push"
  }
}
```

## Have Fun

We hope you enjoy playing with the new features.  As always, jump on our [mailing list](https://groups.google.com/a/ortussolutions.com/forum/#!forum/commandbox), or the [CFML slack team](https://cfml-slack.heroku.com/) with any questions or feedback.  And remember, we provide tools like CommandBox CLI free of charge to the community as [professionally-supported ](https://www.ortussolutions.com/#services)[open source](https://www.ortussolutions.com/#services).   If you have specific needs in the form of features or training for your team, Ortus is here to help you.  [Contact us](https://www.ortussolutions.com/#contact) with any questions.

## Release Notes

### Bug

* \[[COMMANDBOX-347](https://ortussolutions.atlassian.net/browse/COMMANDBOX-347)\] - CFML Function commands do not work on recipes
* \[[COMMANDBOX-348](https://ortussolutions.atlassian.net/browse/COMMANDBOX-348)\] - box update pulling down dev dependencies
* \[[COMMANDBOX-350](https://ortussolutions.atlassian.net/browse/COMMANDBOX-350)\] - Exception in packageservice determining testbox slug runner
* \[[COMMANDBOX-351](https://ortussolutions.atlassian.net/browse/COMMANDBOX-351)\] - Git clone doesn't obey commit hash
* \[[COMMANDBOX-357](https://ortussolutions.atlassian.net/browse/COMMANDBOX-357)\] - tail command doesn't handle CR and LF correclty
* \[[COMMANDBOX-360](https://ortussolutions.atlassian.net/browse/COMMANDBOX-360)\] - Linux: CommandBox 3.1.0-1: Fails to start
* \[[COMMANDBOX-361](https://ortussolutions.atlassian.net/browse/COMMANDBOX-361)\] - Linux distros: /usr/bin/box created with wrong permissions
* \[[COMMANDBOX-367](https://ortussolutions.atlassian.net/browse/COMMANDBOX-367)\] - Progress bar errors if console is too small
* \[[COMMANDBOX-368](https://ortussolutions.atlassian.net/browse/COMMANDBOX-368)\] - Slug auto-complete doesn't work with ForgeBox 2.0
* \[[COMMANDBOX-369](https://ortussolutions.atlassian.net/browse/COMMANDBOX-369)\] - "forgebox search" doesn't work with ForgeBox 2.0
* \[[COMMANDBOX-384](https://ortussolutions.atlassian.net/browse/COMMANDBOX-384)\] - bump command creates invalid version if it starts blank.
* \[[COMMANDBOX-392](https://ortussolutions.atlassian.net/browse/COMMANDBOX-392)\] - CF servers create WEB-INFcfform directory in server root

### New Feature

* \[[COMMANDBOX-77](https://ortussolutions.atlassian.net/browse/COMMANDBOX-77)\] - Start server on any engine
* \[[COMMANDBOX-216](https://ortussolutions.atlassian.net/browse/COMMANDBOX-216)\] - ForgeBox 2 API Integration
* \[[COMMANDBOX-335](https://ortussolutions.atlassian.net/browse/COMMANDBOX-335)\] - forgebox register command
* \[[COMMANDBOX-336](https://ortussolutions.atlassian.net/browse/COMMANDBOX-336)\] - forgebox login command
* \[[COMMANDBOX-337](https://ortussolutions.atlassian.net/browse/COMMANDBOX-337)\] - forgebox publish command
* \[[COMMANDBOX-353](https://ortussolutions.atlassian.net/browse/COMMANDBOX-353)\] - Creation of API Docs for internal CommandBox Core
* \[[COMMANDBOX-354](https://ortussolutions.atlassian.net/browse/COMMANDBOX-354)\] - Update S3 Sync for CommandBox to publish core API Docs
* \[[COMMANDBOX-355](https://ortussolutions.atlassian.net/browse/COMMANDBOX-355)\] - Update the coldbox create command to make the skeleton be a 'name,git+url,http' endpoint
* \[[COMMANDBOX-358](https://ortussolutions.atlassian.net/browse/COMMANDBOX-358)\] - Add command to output system log file
* \[[COMMANDBOX-364](https://ortussolutions.atlassian.net/browse/COMMANDBOX-364)\] - Allow forgebox downloadURL to be any endpoint ID
* \[[COMMANDBOX-371](https://ortussolutions.atlassian.net/browse/COMMANDBOX-371)\] - Allow a package to have listener scripts run by convention
* \[[COMMANDBOX-372](https://ortussolutions.atlassian.net/browse/COMMANDBOX-372)\] - Add pre/postVersion, pre/postPublish interception points
* \[[COMMANDBOX-373](https://ortussolutions.atlassian.net/browse/COMMANDBOX-373)\] - Allow interactive shell \(scripts/recipes\) to have more than one command per line
* \[[COMMANDBOX-374](https://ortussolutions.atlassian.net/browse/COMMANDBOX-374)\] - bump command tags and commits Git repo if present
* \[[COMMANDBOX-376](https://ortussolutions.atlassian.net/browse/COMMANDBOX-376)\] - Global default for server settings
* \[[COMMANDBOX-379](https://ortussolutions.atlassian.net/browse/COMMANDBOX-379)\] - New Icons for Multi-Engine taskbars
* \[[COMMANDBOX-382](https://ortussolutions.atlassian.net/browse/COMMANDBOX-382)\] - Ability to run ad-hoc scripts
* \[[COMMANDBOX-385](https://ortussolutions.atlassian.net/browse/COMMANDBOX-385)\] - Track installs in ForgeBox 2.0 API
* \[[COMMANDBOX-393](https://ortussolutions.atlassian.net/browse/COMMANDBOX-393)\] - Add onServerInstall interception point for addition engine config
* \[[COMMANDBOX-394](https://ortussolutions.atlassian.net/browse/COMMANDBOX-394)\] - Allow server set/show/clear to target a custom JSON file

### Task

* \[[COMMANDBOX-352](https://ortussolutions.atlassian.net/browse/COMMANDBOX-352)\] - Missing 'models' namespace on model test creation
* \[[COMMANDBOX-383](https://ortussolutions.atlassian.net/browse/COMMANDBOX-383)\] - Update Adobe CFEngine wars to have latest updates

### Improvement

* \[[COMMANDBOX-293](https://ortussolutions.atlassian.net/browse/COMMANDBOX-293)\] - Return with exit code 1 when things fail
* \[[COMMANDBOX-338](https://ortussolutions.atlassian.net/browse/COMMANDBOX-338)\] - Add ability to use environment variables to supply java args for BOX itself
* \[[COMMANDBOX-341](https://ortussolutions.atlassian.net/browse/COMMANDBOX-341)\] - install my-module installs unneeded devDependencies
* \[[COMMANDBOX-345](https://ortussolutions.atlassian.net/browse/COMMANDBOX-345)\] - Add ability to specify a server.json by path
* \[[COMMANDBOX-346](https://ortussolutions.atlassian.net/browse/COMMANDBOX-346)\] - Modify build to include sdk format of Unix binary
* \[[COMMANDBOX-349](https://ortussolutions.atlassian.net/browse/COMMANDBOX-349)\] - Improve error message when using "box" from interactive shell
* \[[COMMANDBOX-359](https://ortussolutions.atlassian.net/browse/COMMANDBOX-359)\] - Convert all existing ForgeBox calls to new API format.
* \[[COMMANDBOX-362](https://ortussolutions.atlassian.net/browse/COMMANDBOX-362)\] - Improve messaging and logging when errors connecting to Forgebox
* \[[COMMANDBOX-366](https://ortussolutions.atlassian.net/browse/COMMANDBOX-366)\] - Enhance semver logic for satisfying versions
* \[[COMMANDBOX-370](https://ortussolutions.atlassian.net/browse/COMMANDBOX-370)\] - Allow param completion UDF to see typed text
* \[[COMMANDBOX-375](https://ortussolutions.atlassian.net/browse/COMMANDBOX-375)\] - Capture full java exception stack from Jgit errors
* \[[COMMANDBOX-377](https://ortussolutions.atlassian.net/browse/COMMANDBOX-377)\] - Convert CF Engine downloads to S3/ForgeBox
* \[[COMMANDBOX-378](https://ortussolutions.atlassian.net/browse/COMMANDBOX-378)\] - Fix right click options on server tray icon to be non-Lucee
* \[[COMMANDBOX-380](https://ortussolutions.atlassian.net/browse/COMMANDBOX-380)\] - Allow masking of user input
* \[[COMMANDBOX-381](https://ortussolutions.atlassian.net/browse/COMMANDBOX-381)\] - Auto-correct rewritesEnabled to be rewritesEnable in the start command
* \[[COMMANDBOX-387](https://ortussolutions.atlassian.net/browse/COMMANDBOX-387)\] - Update module scaffolding to create in modules\_app folder.
* \[[COMMANDBOX-388](https://ortussolutions.atlassian.net/browse/COMMANDBOX-388)\] - Make help for commands more intuitive
* \[[COMMANDBOX-389](https://ortussolutions.atlassian.net/browse/COMMANDBOX-389)\] - Don't create init methods for models if included in the method list

\[[COMMANDBOX-390](https://ortussolutions.atlassian.net/browse/COMMANDBOX-390)\] - Switch create controller command to create handler command

