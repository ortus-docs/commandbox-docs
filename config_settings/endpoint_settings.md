# Endpoint Settings

These settings are used to configure CommandBox's endpoints.

## endpoints.forgebox.APIToken
**string**

The API Token provided to you when you signed up for [ForgeBox.io](https://www.forgebox.io/).  This will be set for you automatically when you use the `forgebox register` or `forgebox login` commands.  This token will be sent to ForgeBox to authenticate you.  Please do not share this secret token with others as it will give them permission to edit your packages!
```bash
config set endpoints.forgebox.APIToken=my-very-long-secret-key
config show endpoints.forgebox.APIToken
```


## endpoints.forgebox.APIURL
**string**

This is the URL of the ForgeBox REST API.  Remove this setting to use the default.  If you wish to test submitting package in an environment other than production, you may point to our staging server.  Note, this will funnel ALL ForgeBox calls to the staging server where your APIToken may be different.
```bash
config set endpoints.forgebox.APIURL=https://forgebox.stg.ortussolutions.com/api/v1
config show endpoints.forgebox.APIURL
```
