# Regular Expressions

[https://www.howtogeek.com/661101/how-to-use-regular-expressions-regexes-on-linux/](https://www.howtogeek.com/661101/how-to-use-regular-expressions-regexes-on-linux/)

A regular expression is a filter criterion that can be used to analyze and manipulate strings. It uses letters and symbols to define a pattern that's searched for in a file or stream.

There are several different flavors of regex. We're going to look at the version used in common Linux utilities and commands, like `grep`, the command that [prints lines that match a search pattern](http://man7.org/linux/man-pages/man1/grep.1.html). This is a little different from [using standard regex](https://www.howtogeek.com/devops/how-do-you-actually-use-regex/) in the programming context.

## Grouping Operators

## OR operator

```bash
cry0l1t3@htb:~$ grep -E "(my|false)" /etc/passwd

lxd:x:105:65534::/var/lib/lxd/:/bin/false
pollinate:x:109:1::/var/cache/pollinate:/bin/false
mysql:x:116:120:MySQL Server,,:/nonexistent:/bin/false
```

Since one of the two search parameters always occurs in the three lines, all three lines are displayed accordingly. However, if we use the `AND` operator, we will get a different result for the same search parameters.

## AND operator

```bash
cry0l1t3@htb:~$ grep -E "(my.*false)" /etc/passwd

mysql:x:116:120:MySQL Server,,:/nonexistent:/bin/false
```

Basically, what we are saying with this command is that we are looking for a line where we want to see both `my` and `false`. A simplified example would also be to use `grep` twice and look like this:

```bash
cry0l1t3@htb:~$ grep -E "my" /etc/passwd | grep -E "false"

mysql:x:116:120:MySQL Server,,:/nonexistent:/bin/false
```
