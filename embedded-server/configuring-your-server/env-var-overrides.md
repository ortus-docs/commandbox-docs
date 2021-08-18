# server.json Env Var overrides

Every [server setting](../server.json/) can be overridden by convention by creating environment variables in the shell where you run `box`.  This is idea for CI builds were you want to easily set ports, or tweak settings for your build.  You set set these as actual environment variables or [Java system properties of the CLI](https://commandbox.ortusbooks.com/usage/execution#ad-hoc-java-properties-for-the-cli). 

The var must start with the text `box_server_` and will be followed by the name of the setting.

```bash
box_server_profile=production
```

For nested settings inside of a struct or array you can use underscores to represent dots.

```bash
box_server_web_http_port=8080
```

The overrides are applied using the same mechanism that the `config set` command uses, which means you can also pass JSON directly for complex values.

```bash
# JSON which will be parsed
box_server_web_ssl={ "enabled" : true, "port": 443 }
```

On OS's like Windows which allow for any manner of special characters, you can provide any string which would also be valid for the `config set` command. Ex:

```bash
# dot-delimited keys
box_server_web.http.port=8080
# array indexes too
box_server_web_rules[1]=path-suffix(/box.json) -> set-error(404)
box_server_web_rules[2]=disallowed-methods(trace)
```

When you provide JSON, the `append` flag will be set to true when adding the configuration to what's already in CommandBox.

Overridden env vars will not be written to the `server.json` file and will be lost when box stops. They will also take precedence and override any explicit `server.json` settings already set but will NOT override actual parameters to the `server start` command.

