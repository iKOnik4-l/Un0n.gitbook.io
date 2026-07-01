---
description: >-
  (SQLi Detected (Intercept Web Request > feed it to SQLMap > SQLMap Database
  Enumeration) > RevShell Upload > Local Enumeration > Symblic Link Escalation)
---

# Usage

{% stepper %}
{% step %}
## Scanning and Discovery

![](<../../.gitbook/assets/1HHY6anqeoEckwFFdo bgBQ.png>)

## Website Exploration

![](https://miro.medium.com/v2/resize:fit:1400/1*5pc1gntN_cfCufDRF6DSQ.png)

Recognizing the potential for SQL injection vulnerabilities, we scrutinize the forms for susceptibility. _**After testing each form, the password reset feature exhibits signs of SQL injection vulnerability, evidenced by error responses to single quotes.**_

![](../../.gitbook/assets/1xVtqFCB1pjMQytAJzL0cAA.png)
{% endstep %}

{% step %}
## Exploitation Phase

Employing Burp Suite, we intercept requests to the password reset feature, revealing HTTP POST requests.

![](../../.gitbook/assets/1mbn9T_Ofg18F8Yj9_TJCdg.png)

Utilizing SQLMap, we automate the injection process by **providing the intercepted request in a text file** and execute the following command:

```bash
sqlmap -r request.txt -p email --level 5 --risk 3 --batch --threads 10 --dbs
```

This yields a successful exploitation, divulging the presence of a MySQL database and facilitating further exploration.

![](../../.gitbook/assets/1bdPq1TcKma1EZNaWq0cPtg.png)

sqlmap results
{% endstep %}

{% step %}
## Database Exploitation

We proceed to extract database information using SQLMap, obtaining a list of databases, including “usage\_blog.”

```bash
sqlmap -r request.txt -p email --level 5 --risk 3 --threads 10 -D database_name --tables
```

![](../../.gitbook/assets/1O3OW215jwyNHtnXKw9rsaw.png)

![](../../.gitbook/assets/1zRwxg0QIaegUbAw3uq6BKQ.png)

Continuing our reconnaissance, we extract table information and subsequently retrieve data from the “admin\_users” table.

![](<../../.gitbook/assets/1hy R8cx9_AtyR7h8kUxmfQ.png>)

![](../../.gitbook/assets/1aFNSWIA1VgaLt4fsQ4Xeeg.png)

```bash
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt -show
```

Alternatively, you can use:

```bash
john hash.txt -show
```
{% endstep %}

{% step %}
## User Credentials and Dashboard Access

With obtained credentials, we gain access to the admin dashboard, unveiling insights into the web application’s technologies and versions.

![](../../.gitbook/assets/17MC9AgIgw4QvPG1uGZESuw.png)

Further investigation unveils a potential vulnerability associated with the profile picture upload feature, leading us to exploit it by uploading a PHP reverse shell payload.

[**CVE-2023-24249 : An arbitrary file upload vulnerability in laravel-admin v1.8.19 allows attackers…CVE-2023-24249 : An arbitrary file upload vulnerability in laravel-admin v1.8.19 allows attackers to execute arbitrary…**](https://www.cvedetails.com/cve/CVE-2023-24249/?source=post_page-----16397895490f--------------------------------)

```bash
nc -lvnp [port]
```

Once the reverse shell setup is complete, proceed with the file upload process. Intercept the upload request using Burp Suite to modify the file name back to “.php.jpg.php”. This manipulation ensures that the web server recognizes the uploaded file as a PHP script and executes the reverse shell payload accordingly.

![](../../.gitbook/assets/1W6eg2rvPmFIxbbMK3jJ9mw.png)

![](../../.gitbook/assets/1mwN7a7iwYzcg5iB9YHe20Q.png)
{% endstep %}

{% step %}
## Privilege Escalation Exploration

After uncovering the user flag, our curiosity drives us to delve deeper into the user’s dashboard. Among the intriguing findings, one item piques our interest: the “.monitrc” file. Intrigued by its contents, I decide to explore further, hoping to uncover something of significance.

![](../../.gitbook/assets/1IJ_vbtpQRo59zMdg7OtOHQ.png)

Upon examining the “.monitrc” file, I notice something intriguing. Eager to explore its implications, I opt to initiate an SSH connection to user Xander based on the insights gleaned from the “.monitrc”.

![](../../.gitbook/assets/1iGAYyzBA_vnvjMn7GNjOaQ.png)

![](<../../.gitbook/assets/18giI4ZuLSuF_NkUe xCzbw.png>)

![](../../.gitbook/assets/1mv4SypQQVVm55zsR8SMlCw.png)

Exploring the options within, I focus on the “Project Backup” option. Despite the limited information provided, I recall that this is a custom website. Thus, I decide to explore its default directories, starting with “/var/www/html”.

As our objective is to elevate privileges to root, acquiring the “id\_rsa” file becomes imperative. To achieve this, I execute the following commands within the “/var/www/html” directory:

```bash
touch @id_rsa
ln -s /root/.ssh/id_rsa id_rsa
```

![](../../.gitbook/assets/19GfJTXmuDC4cHxYjCORX2w.png)

![](../../.gitbook/assets/1e4g7kvNem4ldtcfBxdeHqQ.png)
{% endstep %}

{% step %}
## Root Access

Upon obtaining the SSH private key, the next step is exploitation. Here, I copy the hash portion of the private key and save it in my original directory as “id\_rsa”.

Next, I adjust the permissions of the “id\_rsa” file to ensure its confidentiality and integrity:

```bash
chmod 600 id_rsa
```

With the necessary permissions granted, I proceed to establish an SSH connection using the obtained private key:

```bash
ssh -i id_rsa root@IP
```

This command initiates an SSH connection to the target system, utilizing the “id\_rsa” private key for authentication.

![](../../.gitbook/assets/1P9FUCq0kfa6y6ZqcELK3dQ.png)

and we are in.

![](<../../.gitbook/assets/1YRGFox 6ZD FhOZUNhTkuw.png>)
{% endstep %}
{% endstepper %}
