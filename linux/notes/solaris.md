# Solaris 8aad3fda78f64e86886766429e8cd877

The goal of Solaris is to provide a highly stable, secure, and scalable platform for enterprise computing. It has built-in features for high availability, fault tolerance, and system management, making it ideal for mission-critical applications. It is widely used in the banking, finance, and government sectors, where security, reliability, and performance are paramount. It is also used in large-scale data centers, cloud computing environments, and virtualization platforms. Companies such as Amazon, IBM, and Dell use Solaris in their products and services, highlighting its importance in the industry.

## Linux Distributions vs Solaris

Solaris and Linux distributions are two types of operating systems that differ significantly. Firstly, Solaris is a proprietary operating system owned and developed by Oracle Corporation, and its source code is not available to the general public. In contrast, most Linux distributions are open-source, meaning that their source code is available for anyone to modify and use. Additionally, Linux distributions commonly use the Zettabyte File System (`ZFS`), which is a highly advanced file system that offers features such as data compression, snapshots, and high scalability. On the other hand, Solaris uses a Service Management Facility (`SMF`), which is a highly advanced service management framework that provides better reliability and availability for system services.

In terms of package management, Solaris uses the Image Packaging System (`IPS`) package manager, which provides a powerful and flexible way to manage packages and updates. Solaris also provides advanced security features, such as Role-Based Access Control (`RBAC`) and mandatory access controls, which are not available in all Linux distributions.

In summary, the main differences can be grouped into the following categories:

* Filesystem
* Process management
* Package management
* Kernel and Hardware support
* System monitoring
* Security

On Ubuntu, we use the `uname` command to display information about the system, such as the kernel name, hostname, and operating system.

On the other hand, in Solaris, the `showrev` command can be used to display system information, including the version of Solaris, hardware type, and patch level. Here is an example output:

```bash
$ showrev -a

Hostname: solaris
Kernel architecture: sun4u
OS version: Solaris 10 8/07 s10s_u4wos_12b SPARC
Application architecture: sparc
Hardware provider: Sun_Microsystems
Domain: sun.com
Kernel version: SunOS 5.10 Generic_139555-08
```

## Installing Packages

On Ubuntu, the `apt-get` command is used to install packages.

On Solaris, we need to use `pkgadd` to install packages like `SUNWapchr`. The main difference between the two commands is the syntax, and the package manager used. Ubuntu uses the Advanced Packaging Tool (APT) to manage packages, while Solaris uses the Solaris Package Manager (SPM). Also, note that we do not use `sudo` in this case. This is because Solaris used the `RBAC` privilege management tool, which allowed the assignment of granular permissions to users. However, `sudo` has been supported since Solaris 11.

```bash
$ pkgadd -d SUNWapchr
```

## Permission Management

On Linux systems like Ubuntu but also on Solaris, the `chmod` command is used to change the permissions of files and directories.

```bash
JuiceWRLD100@htb[/htb]$ chmod 700 filename
```

```bash
JuiceWRLD100@htb[/htb]$ find / -perm 4000
```

To find files with specific permissions, like with the SUID bit set on Solaris, we can use the `find` command, too, but with a small adjustment.

To find files with specific permissions in Ubuntu, we use the `find` command. Let us take a look at an example of a file with the SUID bit set:

```bash
$ find / -perm -4000
```

## NFS in Solaris

In Solaris, the NFS server can be configured using the `share` command, which is used to share a directory over the network, and it also allows us to specify various options such as read/write permissions, access restrictions, and more. To share a directory over NFS in Solaris, we can use the following command:

```bash
$ share -F nfs -o rw /export/home
```
