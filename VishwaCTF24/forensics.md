## Task 1: Smoke out the Rat
Question: There was a major heist at the local bank. Initial findings suggest that an intruder from within the bank, specifically someone from the bank's database maintenance team, aided in the robbery. This traitor granted access to an outsider, who orchestrated the generation of fake transactions and the depletion of our valuable customers' accounts. We have the phone number, '789-012-3456', from which the login was detected, which manipulated the bank's employee data. Additionally, it's noteworthy that this intruder attempted to add gibberish to the binlog and ultimately dropped the entire database at the end of the heist. Your task is to identify the first name of the traitor, the last name of the outsider, and the time at which the outsider was added to the database. Flag format: VishwaCTF{TraitorFirstName_OutsiderLastName_HH:MM:SS}

Flag: `VishwaCTF{}`

The question gave us a MySQL binary log file. Unfortunately, I could not finish this challenge before the CTF ended, so I attempted it again with help from @rex on Discord.

## Task 2: Repo Riddles
Question: We got a suspicious Linkedin post which got a description and also a zip file with it. It is suspected that a message is hidden in there. Can you find it?

LinkedIn Post Description:

Title: My First Project -The Journey Begins

Hello fellow developers and curious minds!

I'm thrilled to share with you my very first Git project - a labor of love, dedication, and countless late-night commits. 🚀

Explore the code, unearth its nuances, and let me know your thoughts. Your feedback is invaluable and will contribute to the ongoing evolution of this project.

Flag: `VishwaCTF{G1tG1gger_2727}`

The question gave us a zip file that stores a GitHub repository. Unfortunately, I could not finish this challenge before the CTF ended, so I attempted it again with help from @rex on Discord. Inside the hidden .git folder, there are several blobs that could lead us to the flag. Using this [tool](https://github.com/internetwache/GitTools/tree/master), I can extract commits and their content from a broken repository.

```
└─$ sudo bash extractor.sh /vishwa/LearningGit/ /vishwa/Forensics
[sudo] password for kali: 
###########
# Extractor is part of https://github.com/internetwache/GitTools
#
# Developed and maintained by @gehaxelt from @internetwache
#
# Use at your own risk. Usage might be illegal in certain circumstances. 
# Only for educational purposes!
###########
[+] Found commit: 02adccb9209edc074b17967f062cc60413f64293
[+] Found file: /mnt/hgfs/sharedfolder/vishwa/Forensics/0-02adccb9209edc074b17967f062cc60413f64293/index.html
[+] Found file: /mnt/hgfs/sharedfolder/vishwa/Forensics/0-02adccb9209edc074b17967f062cc60413f64293/style.css
[+] Found commit: 41dca9f040deaa65060065ef78523ba44b2c60f1
[+] Found commit: 57f66532b0c6403f95dcfaffa0650f28850d6922
[+] Found commit: 9370bf54bc070fa53c5f2b8b14834db8ed7f3e79
[+] Found commit: aa3306a61a6dc2b4b1fe97ede91c5c843d452c58
[+] Found file: /mnt/hgfs/sharedfolder/vishwa/Forensics/4-aa3306a61a6dc2b4b1fe97ede91c5c843d452c58/f1.txt
[+] Found file: /mnt/hgfs/sharedfolder/vishwa/Forensics/4-aa3306a61a6dc2b4b1fe97ede91c5c843d452c58/f2.txt
[+] Found file: /mnt/hgfs/sharedfolder/vishwa/Forensics/4-aa3306a61a6dc2b4b1fe97ede91c5c843d452c58/f3.txt
[+] Found file: /mnt/hgfs/sharedfolder/vishwa/Forensics/4-aa3306a61a6dc2b4b1fe97ede91c5c843d452c58/f4.txt
[+] Found file: /mnt/hgfs/sharedfolder/vishwa/Forensics/4-aa3306a61a6dc2b4b1fe97ede91c5c843d452c58/f5.txt
[+] Found commit: db01ffe29d0ea57655fcd880dea8816cb2d74d9f
[+] Found file: /mnt/hgfs/sharedfolder/vishwa/Forensics/5-db01ffe29d0ea57655fcd880dea8816cb2d74d9f/f1.txt
[+] Found file: /mnt/hgfs/sharedfolder/vishwa/Forensics/5-db01ffe29d0ea57655fcd880dea8816cb2d74d9f/f2.txt
[+] Found file: /mnt/hgfs/sharedfolder/vishwa/Forensics/5-db01ffe29d0ea57655fcd880dea8816cb2d74d9f/f3.txt
[+] Found file: /mnt/hgfs/sharedfolder/vishwa/Forensics/5-db01ffe29d0ea57655fcd880dea8816cb2d74d9f/f4.txt
[+] Found file: /mnt/hgfs/sharedfolder/vishwa/Forensics/5-db01ffe29d0ea57655fcd880dea8816cb2d74d9f/f5.txt
[+] Found commit: ebf967130d550a180f7e3fda47a5ca96bb442c81
[+] Found file: /mnt/hgfs/sharedfolder/vishwa/Forensics/6-ebf967130d550a180f7e3fda47a5ca96bb442c81/Screenshot 2024-03-01 151511.png
```

Reading each text file manually, the flag is broken up into index strings. So I went ahead to grep the word "string" and got parts of the flag. However, index 6, 7 and 8 was missing from the results obtained.

```
└─$ grep -rni "string" .
./0-02adccb9209edc074b17967f062cc60413f64293/index.html:116:                    string[3: 6] = G1g 
./2-57f66532b0c6403f95dcfaffa0650f28850d6922/commit-meta.txt:6:string[0] = G string[1] = 1 string[2] = t
./4-aa3306a61a6dc2b4b1fe97ede91c5c843d452c58/f4.txt:6:string -- VishwaCTF{}
./4-aa3306a61a6dc2b4b1fe97ede91c5c843d452c58/f4.txt:8:HERE -- 0th Index of string is V.
./5-db01ffe29d0ea57655fcd880dea8816cb2d74d9f/f4.txt:5:string[9] = _
./5-db01ffe29d0ea57655fcd880dea8816cb2d74d9f/f4.txt:6:string[10] = 2
./5-db01ffe29d0ea57655fcd880dea8816cb2d74d9f/f4.txt:7:string[11] = 7
./5-db01ffe29d0ea57655fcd880dea8816cb2d74d9f/f4.txt:8:string[12] = 2
./5-db01ffe29d0ea57655fcd880dea8816cb2d74d9f/f4.txt:9:string[13] = 7
```

After going through the folders again, a png file can be found which contains the letters for index 6, 7 and 8.

![Screenshot 2024-03-01 151511](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/a8dce862-15d5-42a2-a5c8-b672daf04266)

## Task 3: Router |port|
Question: There's some unusual traffic on the daytime port, but it isn't related to date or time requests. Analyze the packet capture to retrieve the flag.

Flag: `VishwaCTF{K3Y5_CAN_0P3N_10CK5}`

We are given a pcap file that has several packets with different protocols. The description mentioned something about daytime port, so I checked online on what port is it to filter my search. Doing some Googling, the port for daytime protocol is `Port 13 (tcp/udp)`. Looking at the packets, there seems to be encoded text that holds valuable information. This is pretty guessy but I tried Vigenère decode with this [site](https://www.guballa.de/vigenere-solver) and it shows that it was decoded using keyword `nnn`.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/67c09a9a-6f6f-4c64-9ad6-499abc3937be)

The decoded text:
```
Hey, mate!
Yo, long time no see! You sure this mode of communication is still safe?
Yeah, unless someone else is capturing network packets on the same network we're using. Anyhow, our text is encrypted, and it would be difficult to interpret. So let's hope no one else is capturing. What's so confidential that you're about to share? It's about cracking the password of a person with the username 'Anonymous.'
Oh wait! Don't you know I'm not so good at password cracking?
Yeah, I know, but it's not about cracking. It's about the analysis of packets. I've completed most of the job, even figured out a way to get the session key to decrypt and decompress the packets. Holy cow! How in the world did you manage to get this key from his device? Firstly, I hacked the router of our institute and closely monitored the traffic, waiting for 'Anonymous' to download some software that requires admin privilege to install. Once he started the download, I, with complete control of the router, replaced the incoming packets with the ones I created containing malicious scripts, and thus gained a backdoor access to his device. The further job was a piece of cake.
Whoa! It's so surprising to see how much you know about networking or hacking, to be specific.
Yeah, I did a lot of research on that. Now, should we focus on the purpose of this meet? Yes, of course. So, what should I do for you?
Have you started the packet capture as I told you earlier?
Yes, I did. Great! I will be sending his SSL key, so find the password of 'Anonymous.' Yes, I would, but I need some details like where to start. The only details I have are he uses the same password for every website, and he just went on to register for a CTF event.
Okay, I will search for it. Wait a second, I won't be sending the SSL key on this Daytime Protocol port; we need to keep this untraceable. I will be sending it through FTP. Since the file is too large, I will be sending it in two parts. Please remember to merge them before using it. Additionally, some changes may be made to it during transfer due to the method I'm using. Ensure that you handle these issues.
Okay! ...
```

Reading the decoded text, it seems that the flag is the password of the user `Anonymous` and the hacker managed to steal a SSL key to encrypt packets. It also mentions that the SSL key is sent to another hacker via FTP. So I filtered the pcap to find ftp-data and found 2 packets, so the key is probably broken up into 2 parts.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/f684a1a6-e4f0-4a77-95b4-6ca3c1b6c351)

Again, I randomly guessed Vigenère with the same site and succesfully extracted both PSK log files and placed them into Wireshark. This can be done by navigating to `Preferences > Protocols > TLS > (Pre)-Master-Secret log files` and add in the key file.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/6f597263-e4e2-4582-8645-e25d13baa080)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/2720f354-161c-4a82-8c7f-98320c98e845)

After decoding it, several HTTP2 and HTTP3 packets can be found. Filtering down for passwords, I managed to find it which is basically the flag.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/e9703380-4f54-4208-8bf4-cd1dfeadcc34)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/0f686fef-22d2-4769-94b4-cebe8f9a43a4)
