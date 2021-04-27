# Task Runner Settings

## taskCaching

**boolean**

Every time you execute a task runner,

* Lucee template cache is cleared
* WireBox's metadata cache is cleared
* Wirebox's mapping for the CFC is unmapped and remapped

This ensure changes take affect right away, but can cause issue under load when you have multi-threaded execution of more than one task at the same time. To skip these cache clearing steps on every run for multi-threaded production use, add the following config setting.

```bash
config set taskCaching=true
```

The setting defaults to **false**.

