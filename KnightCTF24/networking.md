# Scenario: DFIR (Network Analysis)
Recently one of Knight Squad's asset was compromised. We've figured out most but need your help to investigate the case deeply. As a SOC analyst, analyze the pacp file & identify the issues. 

## Task 1: Vicker IP
Question: What is the victim & attacker ip?

Check `Conversations` in Wireshark. The attacker (192.168.1.7) seems to be communicating with our server (192.168.1.8).

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/91c03ff3-19df-47e5-aa5c-7d334d68fda7)

## Task 2: Basic Enum
Question: What tool did the attacker use to do basic enumeration of the server? 

Filter victim or attacker's IP address and slowly analyze the packets, Nikto logs can be found in tcp stream 43.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/1b55e49a-529f-41f6-b23c-06a6313600a6)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/56d5df44-9db3-46e4-af40-b206593411b1)

## Task 3: Vulnerable Service
Question: What service was vulnerable to the main server?

Looking at the packets, suspicious FTP data can be seen being sent. Looking into the ftp stream, it is shown that vFTPd 2.3.4 is indeed being exploited.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/1b55e49a-529f-41f6-b23c-06a6313600a6)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/3a37d86a-6b2e-47ea-9020-409dc1ec9bbc)

## Task 4: CVE ID
Question: What's the CVE id for the vulnerable service?

Just research online on vsFTPd exploits.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/5b926f25-98a8-4218-82a5-0daa4d8132c0)

## Task 5: Famous Tool
Question: The attacker used a popular tool to gain access of the server. Can you name it?

Researching more on the CVE, several video demonstrations shown that the attacker probably used Metasploit to gain access.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/cfae13c3-1712-4bab-9004-3f1b1bb0c33f)

## Task 6: PORT
Question: What was the port number of the reverse shell of the server?

Similarly, the CVE already explains which port is exploits.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/26e4a17b-cfe1-468d-bf5f-53e8b531d4e1)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/e8cf2aec-858b-4814-85cc-571dbb7e1564)

## Task 7: Hidden File
Question: What's the flag of the hidden file?

The flag can be found in the tcp stream previously, however, I could not decode it before the CTF ended. After asking other players on Discord, the true method is twin hex (guessy challenge).

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/d9b6a55e-1343-456b-94d3-c27361c9205f)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/8146c13d-5be8-474f-8690-dee9f5cf5fd3)

## Task 8: Confidential
Question: There's something confidential. Can you find it?

Extract the `confidential.zip` file from Wireshark and within the zip, there is a .docx file. Analyzing the doc file, we can see the flag being somewhere in the file.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/aa3c348f-7d82-41de-8435-52407aca6833)

Pretty straightforward, just resize the image and colorize every text.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/3ee02afd-5c3a-49cb-8769-58e115019dd4)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/20d110bb-3fd9-415b-9e21-31d89cb31748)


## Task 9: BackDoor
Question: What is the backdoor file name?

Analyzing the tcp stream again, we can find a php file being created and renamed. So the real backdoor file is the renamed one.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/c2c92fcb-f879-4e8c-84a5-ed5330ea8fcf)

## Task 10: Super Admin
Question: What is the super admin password in the web application?

We are given a SQL database called `backup.sql` and the root password hash can be found within it. We also know its MD5 from the `process_login.php`.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/2e4dd058-9cd6-4502-a906-6d15a14c52b3)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/ba342348-ff67-4fc1-8bb6-34f5db693a25)

## Task 11: Admin Flag
Question: Can you find the Admin Flag of the web server?

After analyzing everything, I came across the other zip file obtain previous, the `app.zip` file. 
Looking through the app files, we stumble across `dashboard.php` where a seemingly encoded cookie can be found.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/be1e7ed8-ed25-4aee-9004-b0b235d5a717)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/8739626e-73fe-4e72-81f9-1949d92da42c)

## Task 12: Vuln
Question: What was the vulnerability on the edit task page & what parameter was vulnerable?

By following HTTP traffic in the pcap, we can see a supposedly SQL injection attack going on. So we can check the vulnerable source code in `process_edit_task.php` and the parameter can be found.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/cdffad09-f57d-45ac-9837-5c8abeed49a2)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/6629e904-23db-44c0-ad44-a6b338467b91)

So the vulnerablility is SQLi and the parameter is taskId.

## Task 13: Famous Tool 2
Question: What tool did the attacker use to identify the vulnerability of edit task page?  

Check the http stream of the attack, the tool used was SQLMap 1.7.10#stable

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/8e5b15da-88b1-4bef-9797-91de3adf381e)


## Task 14: Something Interesting
Question: What is the super admin password in the web application? 

I had no clue how to solve this, so I asked other members on Discord and they mentioned a suspicious query in the previous SQL database.
I guess it's another guessy challenge, I tried ROT47 and it worked out.

![KNIGHT3](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/4e921ed3-8a1c-49e2-89d3-9b1e5ef2d350)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/27d21b78-6120-4b7b-b12f-8fc685b0b5af)

## Task 15: Hidden Page
Question: There was a hidden page which was only accessible to root & was removed from the web app for security purpose. Can you find it?

A suspicious php file for root can be found in `tasks.php`.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/fb5550d2-e87f-4597-a5d3-a6003088d5d2)


## Task 16: DB Details
Question: What is the database username & databasename?

After spending several minutes on looking at the app files, I actually found the username and password in the previous vFTPd stream.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/826fe007-ee12-403a-a4a3-71d97cf83394)

So the username is actually db_user not root, and the password is kctf2024 as seen in `db.php`.

## Task 17: API Key
Question: What's the API Key?

The API key can be found in `db.php`.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/e353139d-884b-4e02-876b-ce98ffad170d)

