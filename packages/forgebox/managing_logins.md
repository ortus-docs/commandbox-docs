# Managing Logins

You can manage more than one ForgeBox login at a time without needing to re-authenticate.  This can be handy if you have more than one ForgeBox account that you are managing.

## Who am I?
CommandBox will store the API Key for all users that you log in with, however only one is the "active" key. To see what user you are currently using, run the `forgebox whoami` command.

```
forgebox whoami
Brad Wood (bdw429s)
brad@bradwood.com
```

## Switch users
You can switch between the active user with the `forgebox use` command.  
```
forgebox use myUser
forgebox use myOtherUser
```
If you're not already logged in as that user, CommmandBox will prompt you to login.