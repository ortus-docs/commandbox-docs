# Embedded Server

One of the most useful features of CommandBox is the ability to start an ad-hoc server quickly and easily.  Any folder on your hard drive can become the web root of a server.  To start up the server, `cd` into a directory containing some CFML code, and run the `start` command.  An available port will be chosen and in a few seconds, a browser window will open showing the default document (`index.cfm`).

```bash
CommandBox> cd C:\sites\test
CommandBox> start
```