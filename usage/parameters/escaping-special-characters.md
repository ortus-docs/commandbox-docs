# Escaping Special Characters

If a value is a single word with no special characters, you don't need to escape anything. Certain characters are reserved as special characters though for parameters since they demarcate the beginning and end of the actual parameter and you'll need to escape them properly. These rules apply the same to named and positional parameters.

## Spaces

If a parameter has any white space in it, you'll need to wrap the value in single or double quotes. It doesn't matter which kind you use and it can vary from one parameter to another as long as they match properly.

```bash
echo 'Hello World'
echo "Good Morning Vietnam"
```

## Quotes

Quotes are actually allowed unescaped in a value, so long as they don't appear at the start of the string.

```bash
echo O'reilly
```

However, if the parameter contains white space and is surrounded by quotes, you'll need to escape them with a backslash.

```bash
echo 'O\'reilly Auto Parts'
echo "Luis \"The Dev\" Majano"
```

> **Hint** Only like quotes need to be escaped. Single quotes can exist inside of double and vice versa without issue. These examples below are perfectly valid.

```bash
echo "O'reilly Auto Parts"
echo 'Luis "The Dev" Majano'
```

## Backticks

Backticks are used in parameter values to demarcate an expression to be parsed. Escape them with a backslash. All backticks need escaped regardless of whether they are encased on single or double quotes.

```bash
echo "Nothing to \`see\` here"
```

## Equals Signs

If you have an equals sign in your value, you'll need to escape it with a backslash unless you've quoted the entire string.

```bash
echo 2+2\=4
echo "2+2=4"
```

## Line Breaks

Line breaks can't be escaped directly as of Commandbox 4.0. Instead, most terminals let you enter a carriage return by pressing Ctrl-V and pressing enter. To enter a line feed, press Ctrl-V followed by Ctrl-J.

On ConEMU, which performs a paste operation with Ctrl-V, use Ctrl-Shift-V instead.

## Tabs

A tab character can't be escaped directly as of CommandBox 4.0. Instead, most terminals let you enter a tab char by pressing Ctrl-V followed by tab. In ConEMU which allows pasting via Ctrl-V, you can use Ctrl-Shift-V and then press tab.

## Backslash

Since the backslash is used as our escape character you'll need to escape any legitimate backslash that happens to precede a single quote, double quote, equals sign, or letter n.

```bash
echo foo\\\=bar
```

This will print `foo\=bar`

