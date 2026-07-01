# Working with Web Services 1c969e45febb4a71b22a49dff81976bf

There are many different ways to set up web servers on Linux operating systems. One of the most used and widespread web servers, besides IIS and Nginx, is Apache. For an Apache web server, we can use appropriate modules, which can encrypt the communication between browser and web server (`mod_ssl`), use as a proxy server (`mod_proxy`), or perform complex manipulations of HTTP header data (`mod_headers`) and URLs (`mod_rewrite`).

Apache offers the possibility to create web pages dynamically using server-side scripting languages. Commonly used scripting languages are PHP, Perl, or Ruby. Other languages are Python, JavaScript, Lua, and .NET, which can be used for this. We can install the Apache webserver with the following command.

```bash
apt install apache2 -y
```

After we have started it, we can navigate using our browser to the default page ([http://localhost](http://localhost/)).

This is the default page after installation and serves to confirm that the webserver is working correctly.

## CURL

`cURL` is a tool that allows us to transfer files from the shell over protocols like `HTTP`, `HTTPS`, `FTP`, `SFTP`, `FTPS`, or `SCP`.

```bash
curl http://localhost
```

In the title tag, we can see that it is the same text as from our browser. This allows us to inspect the source code of the website and get information from it.

## Wget

An alternative to curl is the tool `wget`. With this tool, we can download files from FTP or HTTP servers directly from the terminal, and it serves as a good download manager. If we use wget in the same way, the difference to curl is that the website content is downloaded and stored locally, as shown in the following example.

```bash
wget http://localhost
```

## Python 3

Another option that is often used when it comes to data transfer is the use of Python 3. In this case, the web server's root directory is where the command is executed to start the server. For this example, we are in a directory where WordPress is installed and contains a "readme.html." Now, let us start the Python 3 web server and see if we can access it using the browser.

```bash
python3 -m http.server
```

We can see what requests were made if we now look at our Python 3 web server's events.

```bash
python3 -m http.server

Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
127.0.0.1 - - [15/May/2020 17:56:29] "GET /readme.html HTTP/1.1" 200 -
127.0.0.1 - - [15/May/2020 17:56:29] "GET /wp-admin/css/install.css?ver=20100228 HTTP/1.1" 200 -
127.0.0.1 - - [15/May/2020 17:56:29] "GET /wp-admin/images/wordpress-logo.png HTTP/1.1" 200 -
127.0.0.1 - - [15/May/2020 17:56:29] "GET /wp-admin/images/wordpress-logo.svg?ver=20131107 HTTP/1.1" 200 -
```

## Practical Juice

Find a way to start a simple HTTP server inside Pwnbox or your local VM using "npm". Submit the command that starts the web server on port 8080 (use the short argument to specify the port number).

```bash
http-server -p 8080
```

Find a way to start a simple HTTP server inside Pwnbox or your local VM using "php". Submit the command that starts the web server on the localhost (127.0.0.1) on port 8080.

```bash
php -S 127.0.0.1:8080
```
