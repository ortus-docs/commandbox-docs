# Creating a user
Browsing the packages in ForgeBox and installing them can be done without being authenticated, but to publish your own packages, you'll need a Forgebox user.

## Creating a User from CommandBox

To create a new ForgeBox user right from the CLI run the "forgebox register" command.

## Authenticating from CommandBox

Once you have a confirmed user, you can run "forgebox login" to authenticate your account.  This will store your API key in a CommandBox config setting.  You can see it with:

```bash
CommandBox> config show endpoints.forgebox
```


