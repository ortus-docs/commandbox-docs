# Env Var Overrides

Every [Config Setting](./) can be overridden by convention by creating environment variables in the shell where you run `box`.  This is idea for CI builds were you want to easily set ForgeBox API keys, or tweak settings for your build.  You set set these as actual environment variables or [Java system properties of the CLI](https://commandbox.ortusbooks.com/usage/execution#ad-hoc-java-properties-for-the-cli).&#x20;

The var must start with the text `box_config_` and will be followed by the name of the setting.

```bash
box_config_colorInDumbTerminal=true
```

For nested settings inside of a struct or array you can use underscores to represent dots.

```bash
box_config_endpoints_forgebox_APIToken=my-token-here
```

The overrides are applied using the same mechanism that the `config set` command uses, which means you can also pass JSON directly for complex values.

```bash
# JSON which will be parsed
box_config_proxy={ "server" : "localhost", "port": 80 }
```

On OS's like Windows which allow for any manner of special characters, you can provide any string which would also be valid for the `config set` command. Ex:

```bash
# dot-delimited keys
box_config_endpoints.forgebox.APIToken=my-token-here
# array indexes too
box_config_foo.bar[baz].bum[1]=test
```

When you provide JSON, the `append` flag will be set to true when adding the configuration to what's already in CommandBox.

Overridden env vars will not be written to the `CommandBox.json` file and will be lost when box stops. They will also take precedence and override any explicit settings already set.
