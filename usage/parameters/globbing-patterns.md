# Globbing Patterns

When a command has an argument with a type of `Globber` for a file path, that means you can use file globbing patterns to affect more than one file at a time. Globbing patterns are common in Bash as well as places like your `.gitignore` file. They use common wildcard patterns to provide a partial path that can match zero or hundreds of files all at the same time.

## "?" matches a single character

If a globbing pattern contains a question mark, that will match any single character. So a pattern of `ca?.txt` would match `car.txt`, and `cat.txt`, but not `cart.txt`. You can use a wildcard more than once. `p?p?.cf?` would match files named `papa.cfm` and `pipe.cfc`.

## "\*" matches any number of characters within name

If a globbing pattern contains a single asterisks, that will match zero or more characters inside a filename or folder name. So `d*o` matches `doodoo`, `dao`, and just `do`. The wildcard only counts _inside_ a file or folder name, so `models/*.cfc` will only match cfc files in the root of the models folder.

## "\*\*" matches any number of characters across all directories

To extend the previous example, if we did `models/**.cfc` that would match any cfc file in _any_ subdirectory, no matter how deep.

## Globbing examples

Here's some examples of what file globbing might look like:

```text
CommandBox> rm temp*.txt
CommandBox> cp *.cfm backup/
CommandBox> touch build/*.properties
```

Here's some more examples of how the wildcards work

```text
// Match any file or folder starting with "foo"
foo*

// Match any file or folder starting with "foo" and ending with .txt
foo*.txt

// Match any file or folder ending with "foo"
*foo 

// Match a/b/z but not a/b/c/z
a/*/z

// Match a/z and a/b/z and a/b/c/z
a/**/z

// Matches hat but not ham or h/t
/h?t
```

Since the Globber library can handle more than one globbing pattern, any command that uses a Globber type can accept a comma-delimited list of patterns.  The following will list any .cfm AND .md files in the directory.

```bash
dir *.cfm,*.md
```



