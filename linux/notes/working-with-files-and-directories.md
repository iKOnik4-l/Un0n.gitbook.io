# Working with Files and Directories cdd03268c37147eb889f00b57f6d8d90

The terminal in Linux is a more efficient and faster tool because you can access files directly with a few commands and edit and modify them selectively with regular expressions (`regex`).

We can use `touch` to create an empty file and `mkdir` to create a directory.

```bash
JuiceWRLD100@htb[/htb]$ touch <name>
```

```bash
JuiceWRLD100@htb[/htb]$ mkdir <name>
```

**We may want to have specific directories in the directory**, and it would be very time-consuming to create this command for every single directory. The command `mkdir` has an option marked `-p` to add parent directories.

```bash
JuiceWRLD100@htb[/htb]$ mkdir -p Storage/local/user/documents
```

```bash
JuiceWRLD100@htb[/htb]$ tree ..
├── info.txt
└── Storage
    └── local
        └── user
            └── documents

4 directories, 1 file
```

We can also create files directly in directories by specifying the path where the file should be stored. The trick is to use the single dot (`.`) to tell the system that we want to start from the current directory. So the command for creating another empty file looks like this:

```bash
JuiceWRLD100@htb[/htb]$ touch ./Storage/local/user/userinfo.txt
```

```bash
JuiceWRLD100@htb[/htb]$ tree ..
├── info.txt
└── Storage
    └── local
        └── user
            ├── documents
            └── userinfo.txt

4 directories, 2 files
```

With the command `mv`, we can move and also rename files and directories. The syntax for this looks like this:

```bash
JuiceWRLD100@htb[/htb]$ mv <file/directory> <renamed file/directory>
```

```bash
JuiceWRLD100@htb[/htb]$ mv info.txt information.txt
```

**Move it to the directory `Storage`.**

```bash
JuiceWRLD100@htb[/htb]$ mv information.txt readme.txt Storage/
```

```bash
JuiceWRLD100@htb[/htb]$ tree ..
└── Storage
    ├── information.txt
    ├── local
    │   └── user
    │       ├── documents
    │       └── userinfo.txt
    └── readme.txt

4 directories, 3 files
```
