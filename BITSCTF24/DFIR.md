# Solution
## Task 1: Intro to DFIR
Scenario: DFIR or Digital Forensics and Incident Response is a field within cybersecurity that focuses on the identification, investigation, and remediation of cyberattacks. Here are the types of analysis you can expect throughout these sequence of challenges!

In this DFIR challenge, we have a memory dump, a AD1 image file and a pcap file. The flag is given directly in the question.

Flag: `BITSCTF{DFIR_r0ck55}`

## Task 2: Access Granted!
Scenario: First things first. MogamBro is so dumb that he might be using the same set of passwords everywhere, so lets try cracking his PC's password for some luck.

I started the search with the memory dump first. Since we are looking for a password, we can use the windows.hashdump plugin in Vol3 to extract the NTLM hashes and crack MogamBro's password hash.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/0f7f2d2e-a922-447b-a73f-2d060980ede6)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/bab07dbf-c5f8-4cbd-bc55-6af1bd2b7e63)

Flag: `BITSCTF{adolfhitlerrulesallthepeople}`

## Task 3: 0.69 Day
Scenario: MogamBro was using some really old piece of software for his daily tasks. What a noob! Doesn't he know that using these deprecated versions of the same leaves him vulnerable towards various attacks! Sure he faced the consequences through those spam mails. Can you figure out the CVE of the exploit that the attacker used to gain access to MogamBro's machine & play around with his stuff.


Flag: `BITSCTF{}`

## Task 4: I'm wired in
Scenario: MogamBro got scared after knowing that his PC has been hacked and tried to type a SOS message to his friend through his 'keyboard'. Can you find the contents of that message, obviously the attacker was logging him!

Since the question mentioned something about his keyboard, I assume we have to find the keystrokes or any history of the SOS message. Going through the AD1 image, I noticed a keylog.pcapng file in the Desktop directory of MogamBro.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/03724447-1486-40cf-98cb-a4102e25788b)

After extracting and analyzing the pcapng file, USB packets can be found, suggesting that this could be the keyboard packets. Reading online about USB packets, I found this [blog](https://medium.com/@sidharthpanda1/usb-sniffer-packet-challenge-cryptoverse-ctf-forensics-c42f2975e8f1) that talks about parsing HID data from the USB packets to extract the keystrokes. Following the blog, I added HID data as a column and exported the packets to a csv file.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/d0c80b8b-c0d4-4011-8ac2-7d458040e13f)

With the csv file, I can then extract the HID data as mentioned in the blog. I researched online to find a way to decode the keystrokes and found this incredible [tool](https://github.com/syminical/PUK). Using the tool, I got the flag.

```
cat hiddata.csv | cut -d “,” -f 7 | cut -d “\”” -f 2 | grep -vE “HID Data” > hexoutput.txt

0200000000000000
02000c0000000000
0200000000000000
0000000000000000
00002c0000000000
0000000000000000
00000b0000000000
...
```

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/5f63a60f-f809-4ed3-af65-858799e7e03b)

However, the flag seems to be incorrect. Then i noticed that there is an extra '-' placed into the flag.

Flag: `BITSCTF{BITSCTF{I_7h1nk_th3y_4Re_k3yl0991ng_ME!}`
