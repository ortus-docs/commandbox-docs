# Escaping Params from the OS Shell

Commmand is one of the only CLIs to offer named parameters, which are intuitive and self-documenting.  When using the `box` binary from your OS's native shell, you may find an unexpected error like so:

```bash
C:\>box echo text="Hello world"
ERROR: Please don't mix named and positional parameters, it makes me dizzy.
echo text=Hello world
```

Using a positional parameter won't error, but "world" disappears:

```bash
C:\>box echo "Hello world"
Hello
```

What happened there?  You have a space in the value of you `text` parameter, but you escaped it by wrapping the value in quotes!

## Nom Nom
The problem is that the command is parsed twice-- once by your operating system and again by CommandBox and the OS ate the quotes!  The text below the error message shows what the command looked like when it reached CommandBox (sans quotes)

```bash
echo text=Hello world
```
That looks like two parameters:
* `text=Hello`
* `world`

The fix for this is simple.  You need to double-escape the command to placate both the OS and CommandBox's hunger for tasty quotes.  Technically you can just wrap the quoted portion in an extra set of quotes, but it might be easier to just wrap the entire bit after `box` like so:

```bash
C:\>box "echo text=\"Hello world\""
```

Or positionally:

```bash
C:\>box "echo \"Hello world\""
```

Your operating system will munch the outer set of quotes, and hand what is left to CommandBox with the inner set still in tact.  

#When to bother

If you are using CommandBox from its interactive shell, you needn't worry about this.  In the interactive shell, what you type is directly processed by the CommandBox parser.  Even when working in your native OS shell, you won't generally have to worry about this for simple commands.  The rule of thumb is if you need to wrap a string in quotes from your native OS shell, make sure you escape it twice.





