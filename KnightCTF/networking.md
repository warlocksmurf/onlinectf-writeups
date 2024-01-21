# Scenario
Recently one of Knight Squad's asset was compromised. We've figured out most but need your help to investigate the case deeply. As a SOC analyst, analyze the pacp file & identify the issues. 

# Solution
## Task 1: Vicker IP
Question: What is the victim & attacker ip?
Check `Conversations` in Wireshark.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/91c03ff3-19df-47e5-aa5c-7d334d68fda7)

## Task 2: Basic Enum
Question: What tool did the attacker use to do basic enumeration of the server? 
Filter victim or attacker's IP address and slowly analyze the packets. We can find Nikto logs in stream 43.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/1b55e49a-529f-41f6-b23c-06a6313600a6)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/56d5df44-9db3-46e4-af40-b206593411b1)

## Task 3: Vulnerable Service
Question: What service was vulnerable to the main server?
We can notice suspicious FTP data being sent around. Looking into the stream, we can notice the service being exploited with its version.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/1b55e49a-529f-41f6-b23c-06a6313600a6)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/3a37d86a-6b2e-47ea-9020-409dc1ec9bbc)

## Task 4: CVE ID
Question: What's the CVE id for the vulnerable service?
Just look online on vsFTPd exploits.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/5b926f25-98a8-4218-82a5-0daa4d8132c0)

## Task 5: Famous Tool
Question: The attacker used a popular tool to gain access of the server. Can you name it?
Researching more on the CVE, we can understand the attacker probably used Metasploit as shown in this video demonstration.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/cfae13c3-1712-4bab-9004-3f1b1bb0c33f)

## Task 6: PORT
Question: What was the port number of the reverse shell of the server?
Similarly, the CVE already explains which port is exploits.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/26e4a17b-cfe1-468d-bf5f-53e8b531d4e1)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/e8cf2aec-858b-4814-85cc-571dbb7e1564)

## Task 7: Hidden File
Question: What's the flag of the hidden file?
The flag can be found in the tcp stream previously, however, I could not decode it before the CTF ended. After asking other players on Discord, the true method is twin hex.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/d9b6a55e-1343-456b-94d3-c27361c9205f)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/8146c13d-5be8-474f-8690-dee9f5cf5fd3)

## Task 8: Confidential
Question: There's something confidential. Can you find it?
Extract HTTP objects especially the confidential zip file, within the zip has a .docx file. Analyzing the doc file, we can see the flag being somewhere in the file.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/aa3c348f-7d82-41de-8435-52407aca6833)

Pretty straightforward, just resize the image and colorize every text.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/3ee02afd-5c3a-49cb-8769-58e115019dd4)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/20d110bb-3fd9-415b-9e21-31d89cb31748)


## Task 9: BackDoor
Question: What is the backdoor file name?
Analyzing the tcp stream again, we can find a php file being creaated and renamed. So the real backdoor file is the renamed one.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/c2c92fcb-f879-4e8c-84a5-ed5330ea8fcf)

## Task 10: Super Admin
Question: What is the super admin password in the web application?


## Task 14: Something Interesting
Question: What is the super admin password in the web application? 
I had no clue how to solve this, so I asked other members on Discord and they mentioned a suspicious query in the previous SQL database. 
I guess it's another guessy challenge, I tried ROT47 and it worked out.

![KNIGHT3](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/4e921ed3-8a1c-49e2-89d3-9b1e5ef2d350)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/27d21b78-6120-4b7b-b12f-8fc685b0b5af)


