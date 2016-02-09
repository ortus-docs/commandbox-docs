Make sure gitbook cli is installed

```
npm install gitbook-cli -g
```

Create the directories and files for a book from its `SUMMARY.md` file (if existing) using

```
$ gitbook init
```

You can serve a repository as a book using:

```
$ gitbook serve
```

Or simply build the static website using:

```
$ gitbook build
```

## Other Formats

> You need to have ebook-convert installed

```
gitbook pdf ./myrepo ./mybook.pdf
gitbook epub ./myrepo ./mybook.epub
gitbook mobi ./myrepo ./mybook.mobi
gitbook build ./myrepo --format=json
```