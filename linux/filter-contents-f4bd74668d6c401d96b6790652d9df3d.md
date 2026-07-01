# Filter Contents f4bd74668d6c401d96b6790652d9df3d

To read files, we do not necessarily have to use an editor for that. There are two tools called `more` and `less`, which are very identical. These are fundamental `pagers` that allow us to scroll through the file in an interactive view. Let us have a look at some examples.

## More

```bash
more /etc/passwd
```

**Why use More**

When you are new to Linux, you try to [use the cat command](https://linuxhandbook.com/cat-command/) all the time to read the content of a file. This works great for files with only a few lines out of output, but larger files quickly scroll content past the user making it difficult, or even impossible for you to find what you need.

The more command opens a text file in page views. You can read page by page and when you quit more, there will be no output visible on the screen. Your terminal will be clean and pristine.

| **Input**              | **Action**                                      |
| ---------------------- | ----------------------------------------------- |
| Down Arrow / Space Bar | Scroll Down (One Page)                          |
| Up Arrow / B           | Scroll Up (One Page)                            |
| Enter                  | Scroll One Line at a Time                       |
| – number               | The Number of Lines per Screenful               |
| + number               | Display File Beginning from Line Number         |
| +/ string              | Display File Beginning from Search String Match |
| q                      | Quit viewing the text file and return to screen |

## Less

If we now take a look at the tool [less](https://linuxhandbook.com/less-command/), we will notice on the man page that it contains many more features than `more`.

```bash
less /etc/passwd
```

With less, you can read large text files without cluttering your terminal screen. You can also search for text and monitor files in real time with it.

Some people prefer [using Vim](https://linuxhandbook.com/basic-vim-commands/) for reading large text files. But less is faster than Vim or other such text editors because it doesn’t read the entire file before starting. Since less is ‘read only‘, you don’t have the risk of accidentally editing the files you are viewing.

## Head

If we only want to get the `first` lines of the file, we can use the tool `head`. By default, `head` prints the first ten lines of the given file or input, if not specified otherwise.

## Tail

If we only want to see the last parts of a file or results, we can use the counterpart of `head` called `tail`, which returns the `last` ten lines.

## Sort

Depending on which results and files are dealt with, they are rarely sorted. Often it is necessary to sort the desired results alphabetically or numerically to get a better overview. For this, we can use a tool called `sort`.

## Grep

More often, we will only search for specific results that contain patterns we have defined. One of the most used tools for this is `grep`, which offers many different features. Accordingly, we can search for users who have the default shell `"/bin/bash"` set as an example.

```bash
cat /etc/passwd | grep "/bin/bash"

root:x:0:0:root:/root:/bin/bash
mrb3n:x:1000:1000:mrb3n:/home/mrb3n:/bin/bash
cry0l1t3:x:1001:1001::/home/cry0l1t3:/bin/bash
htb-student:x:1002:1002::/home/htb-student:/bin/bash
```

## Cut

Specific results with different characters may be separated as delimiters. Here it is handy to know how to remove specific delimiters and show the words on a line in a specified position. One of the tools that can be used for this is `cut`. Therefore we use the option `-d` and set the delimiter to the colon character (`:`) and define with the option `-f` the position in the line we want to output.

```bash
cat /etc/passwd | grep -v "false\|nologin" | cut -d":" -f1

root
sync
mrb3n
cry0l1t3
htb-student
```

## Tr

Another possibility to replace certain characters from a line with characters defined by us is the tool `tr`. As the first option, we define which character we want to replace, and as a second option, we define the character we want to replace it with. In the next example, we replace the colon character with space.

```bash
cat /etc/passwd | grep -v "false\|nologin" | tr ":" " "

root x 0 0 root /root /bin/bash
sync x 4 65534 sync /bin /bin/sync
mrb3n x 1000 1000 mrb3n /home/mrb3n /bin/bash
cry0l1t3 x 1001 1001  /home/cry0l1t3 /bin/bash
htb-student x 1002 1002  /home/htb-student /bin/bash
```

## Column

Since search results can often have an unclear representation, the tool `column` is well suited to display such results in tabular form using the `-t`.

```bash
cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | column -t

root         x  0     0      root               /root        /bin/bash
sync         x  4     65534  sync               /bin         /bin/sync
mrb3n        x  1000  1000   mrb3n              /home/mrb3n  /bin/bash
cry0l1t3     x  1001  1001   /home/cry0l1t3     /bin/bash
htb-student  x  1002  1002   /home/htb-student  /bin/bash
```

## Awk

As we may have noticed, the user `postgres` has one row too many. To keep it as simple as possible to sort out such results, the (`g`)`awk` programming is beneficial, which allows us to display the first (`$1`) and last (`$NF`) result of the line.

```bash
cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}'

root /bin/bash
sync /bin/sync
mrb3n /bin/bash
cry0l1t3 /bin/bash
htb-student /bin/bash
```

## Sed

There will come moments when we want to change specific names in the whole file or standard input. One of the tools we can use for this is the stream editor called `sed`. One of the most common uses of this is substituting text. Here, `sed` looks for patterns we have defined in the form of regular expressions (regex) and replaces them with another pattern that we have also defined. Let us stick to the last results and say we want to replace the word `bin` with `HTB`.

The `s` flag at the beginning stands for the substitute command. Then we specify the pattern we want to replace. After the slash (`/`), we enter the pattern we want to use as a replacement in the third position. Finally, we use the `g` flag, which stands for replacing all matches.

```bash
cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}' | sed 's/bin/HTB/g'

root /HTB/bash
sync /HTB/sync
mrb3n /HTB/bash
cry0l1t3 /HTB/bash
htb-student /HTB/bash
```

## Wc

Last but not least, it will often be useful to know how many successful matches we have. To avoid counting the lines or characters manually, we can use the tool `wc`. With the `-l` option, we specify that only the lines are counted.

```bash
cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}' | wc -l

5
```

## Practical Juice

How many services are listening on the target system on all interfaces? (Not on localhost and IPv4 only)

```bash
ss -l -4 | grep -v "127\.0\.0" | grep "LISTEN" | wc -l
```

Where:

* **l**: show only listening services
* **4**: show only ipv4
* **grep -v "127.0.0"**: exclude all localhost results
* **grep "LISTEN"**: better filtering only listening services
* **wc -l**: count results

Determine what user the ProFTPd server is running under. Submit the username as the answer.

```bash
cd /etc/proftpd/
```

```bash
less proftpd.conf
```

Use cURL from your Pwnbox (not the target machine) to obtain the source code of the "[https://www.inlanefreight.com](https://www.inlanefreight.com/)" website and filter all unique paths of that domain. Submit the number of these paths as the answer.

```bash
wget --spider --recursive https://www.inlanefreight.com
```

```
Found 10 broken links.

https://www.inlanefreight.com/wp-content/themes/ben_theme/fonts/glyphicons-halflings-regular.svg
https://www.inlanefreight.com/wp-content/themes/ben_theme/fonts/glyphicons-halflings-regular.eot
https://www.inlanefreight.com/wp-content/themes/ben_theme/images/testimonial-back.jpg
https://www.inlanefreight.com/wp-content/themes/ben_theme/css/grabbing.png
https://www.inlanefreight.com/wp-content/themes/ben_theme/fonts/glyphicons-halflings-regular.woff
https://www.inlanefreight.com/wp-content/themes/ben_theme/fonts/glyphicons-halflings-regular.woff2
https://www.inlanefreight.com/wp-content/themes/ben_theme/images/subscriber-back.jpg
https://www.inlanefreight.com/wp-content/themes/ben_theme/fonts/glyphicons-halflings-regular.eot?
https://www.inlanefreight.com/wp-content/themes/ben_theme/images/fun-back.jpg
https://www.inlanefreight.com/wp-content/themes/ben_theme/fonts/glyphicons-halflings-regular.ttf

FINISHED --2020-12-06 05:34:58--
Total wall clock time: 2.5s
Downloaded: 23 files, 794K in 0.1s (5.36 MB/s)
```

Now, assuming 23 downloads and 10 broken links all add up to be the unique path I got 33 and it was the correct answer.
