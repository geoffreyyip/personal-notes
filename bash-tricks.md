## Globs

They're like regular expressions, except they're not. CLIs aren't capable of parsing regular expressions.

### Basics

`*` matches 0 or more characters. For example, `*.js` would match all Javascript files in the current directory. Its regex equivalent is `.*`

`?` matches precisely one character. For example, `notes?*.js` would match `notes1.js`, `notes2.js`, and `notes3.js`. Its regex equivalent is `.`

`[]` is known as a *range* or *class*. Globs will match exactly one of the listed characters. For example, `notes[1,2]` will match `notes1.js` and `notes2.js` but not `notes3.js`.

`**` is a `globstar` feature added for Bash 4.0. It matches zero of more directories. It is equivalent to `* */* */*/* */*/*` are so forth until it's hit the last directory.

### Extended Globs: Pattern Lists

`?(pattern-list)` matches zero or one occurrence of the given patterns.

`*(pattern-list)` matches zero or more occurrences of the given patterns.

`+(pattern-list)` matches one or more occurrences of the given patterns.

`@(pattern-list)` matches exactly one of the given patterns.

`!(pattern-list)` matches anything except one of the given patterns.

`pattern-list` is pipe-delimited. For example, `!(01|02).mp3` matches all songs except for `01.mp3` and `02.mp3`

### Other Helpful Tricks

`{}` is known as `brace expansion`. It's native to the shell and not part of glob syntax. But it's helpful to know. It expands a parameter into multiple parameters. For example, the following statements are equivalent:

```sh
$ mkdir /home/bash/test/{foo,bar,baz}
```

```shell
$ mkdir /home/bash/test/foo /home/bash/test/bar /home/bash/test/baz
```

You can also do sequences `{1..5}` expands to `1 2 3 4 5`. Note that its two dots, not the three-dot ellipsis.

Be careful with `brace expansion`. It works at a very low level and may not match to what you think it should do.



 