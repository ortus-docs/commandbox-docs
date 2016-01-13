# User Module Settings

The defaults for a module is stored in a `settings` struct in the `ModuleConfig.cfc` for that module.  However, users can override those settings without needing to touch the core module code.  This is made possible by special namespace in the CommandBox config settings called `modules`.

Consider the following module settings:

**`ModuleConfig.cfc`**

