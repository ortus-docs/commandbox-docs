# Github Actions

There are many ways to install CommandBox in your Github actions. However, we have created the official Github Action package so you can install CommandBox easily: [_Setup CommandBox Action_](https://github.com/Ortus-Solutions/setup-commandbox)

{% embed url="https://github.com/Ortus-Solutions/setup-commandbox" %}
Setup CommandBox Action
{% endembed %}

{% embed url="https://github.com/marketplace/actions/setup-commandbox-cli" %}
Githb Action Marketplace
{% endembed %}

### Inputs

The following are all the different input variables you can use on the action so you can setup CommandBox with ForgeBox API keys, default packages, specific versions and much more.

<table><thead><tr><th width="266">Input</th><th width="150">Type</th><th>Default</th><th>Description</th></tr></thead><tbody><tr><td><code>forgeboxAPIKey</code></td><td>string</td><td>---</td><td>If added to the action, we will seed it in CommandBox for you.</td></tr><tr><td><code>installSystemModules</code></td><td>boolean</td><td><code>false</code></td><td>If true then it will install: <code>commandbox-cfconfig, commandbox-dotenv</code> for you</td></tr><tr><td><code>install</code></td><td>string</td><td>---</td><td>If added, a comma-delmitted list of packages to install upon installation of the binary for you.</td></tr><tr><td><code>warmup</code></td><td>boolean</td><td><code>false</code></td><td>If true and no install inputs detected, it will run the box binary.</td></tr><tr><td><code>version</code></td><td>semver</td><td><code>latest</code></td><td>The CommandBox version to install, if not passed we use the latest stable.</td></tr></tbody></table>



### Usage

Simple usage:

```yaml
- name: Setup CommandBox
  uses: Ortus-Solutions/setup-commandbox@v2.0.0
```

With Global Dependencies:

```yaml
- name: Setup CommandBox
  uses: Ortus-Solutions/setup-commandbox@v2.0.0
  with:
    installSystemModules: true
```

With Specific Dependencies:

```yaml
- name: Setup CommandBox
  uses: Ortus-Solutions/setup-commandbox@v2.0.0
  with:
    install: commandbox-fusionreactor
```

With ForgeBox Token

```yaml
- name: Setup CommandBox With ForgeBox Key
  uses: Ortus-Solutions/setup-commandbox@v2.0.0
  with:
    forgeboxAPIKey: my-token
```

Install a specific version of CommandBox

```yaml
- name: Setup CommandBox With ForgeBox Key
  uses: Ortus-Solutions/setup-commandbox@v2.0.0
  with:
    version: 5.0.0
```
