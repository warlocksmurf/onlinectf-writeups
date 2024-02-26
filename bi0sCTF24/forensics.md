# Challenge 1: verboten
## Scenario
Randon, an IT employee finds a USB on his desk after recess. Unable to contain his curiosity he decides to plug it in. Suddenly the computer goes haywire and before he knows it, some windows pops open and closes on its own. With no clue of what just happened, he tries seeking help from a colleague. Even after Richard’s effort to remove the malware, Randon noticed that the malware persisted after his system restarted.

We are given an .ad1 file for our investigation which can be analyzed using FTK Imager.

### Q1: What is the serial number of the sandisk usb that he plugged into the system? And when did he plug it into the system? Format: verboten{serial_number:YYYY-MM-DD-HH-MM-SS}
Flag: `verboten{4C530001090312109353&0:2024-02-16-12-01-57}`

I remember learning about USB registries from other CTFs, so I did some research on what registries I should look out for and this ![blog](https://www.cybrary.it/blog/usb-forensics-find-the-history-of-every-connected-usb-device-on-your-computer) was what I was looking for. Basically, the information can be found in `SYSTEM\CurrentControlSet\Enum\USBSTOR`.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/41d73b7c-8951-4765-9e36-862131dc8a52)

### Q2: What is the hash of the url from which the executable in the usb downloaded the malware from? Format: verboten{md5(url)}
Flag: `verboten{11ecc1766b893aa2835f5e185147d1d2}`

Reading the question, several keywords can be identified: url and downloaded. Hence, the answer is most likely in Randon's browser history. Looking at the user's directories, it seems that Google should be the browser used to download the malware.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/35aa060d-42f2-485d-8dac-a1696aba0ffb)

After extracting and analyzing the `History` artifact with DB Browser, the URL can be identified as `https://filebin.net/qde72esvln1cor0t/mal`.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/cd38d044-c13d-4693-af29-bb5947eb7f93)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/6079de11-e961-4745-a256-a8b438ff7525)

### Q3: What is the hash of the malware that the executable in the usb downloaded which persisted even after the efforts to remove the malware? Format: verboten{md5{malware_executable)}
Flag: `verboten{169cbd05b7095f4dc9530f35a6980a79}`

Since the malware was already identified in the previous question, we just have to look for `mal.exe` in Randon's machine. After going through several common directories, the malware can be found in `C:\Users\randon\AppData\Roaming\Microsoft\Windows\Startup\`

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/fb94da6b-03ac-41bb-9931-bac373cef4d6)

### Q4: What is the hash of the zip file and the invite address of the remote desktop that was sent through slack? Format: verboten{md5(zip_file):invite_address}
Flag: `verboten{b092eb225b07e17ba8a70b755ba97050:1541069606}`

Reading the question, it mentions Slack (a messaging app that can be installed in Windows and mobile devices). Analyzing Randon's machine, the Slack directory can be found in `C:\Users\randon\AppData\Roaming\`. The first thing I analyzed was the `root-state.json` file located in `C:\Users\randon\AppData\Roaming\Slack\storage\`. Using a JSON beautifier tool, it shows the name of the zip file clearly which is `file_shredder.zip`.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/fee0b2f4-6574-434e-8c4d-b97b5b929f90)

Reading on how I can extract cached files in Slack, I found this ![blog](https://medium.com/@jeroenverhaeghe/forensics-finding-slack-chat-artifacts-d5eeffd31b9c) that mentions using ![Nirsoft Chrome Cache Viewer](https://www.nirsoft.net/utils/chrome_cache_view.html) to view cached data in Slack. By filtering the name of the zip file, we can find the MD5 hash of the zip in the ETag column.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/95c76b1c-7992-4534-9d88-e46852b21bd5)

Reading the blog again, I analyze the IndexedDB blob file (`https_app.slack.com_0.indexeddb.blob`) to maybe find the invite address. Using grep, the invite address can be found with a suspicious text about AnyDesk connection: `here is the address for anydesk: 1541069606`.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/c1ae7f39-9d05-448b-8e95-c5ea6ca4a1ad)

### Q5: What is the hash of all the files that were synced to Google Drive before it was shredded? Format: verboten{md5 of each file separated by ':'}
Flag: `verboten{ae679ca994f131ea139d42b507ecf457:4a47ee64b8d91be37a279aa370753ec9:870643eec523b3f33f6f4b4758b3d14c:c143b7a7b67d488c9f9945d98c934ac6:e6e6a0a39a4b298c2034fde4b3df302a}`

Doing some research online, I found this interesting ![blog](https://amgedwageh.medium.com/drivefs-sleuth-investigating-google-drive-file-streams-disk-artifacts-0b5ea637c980) about Google Drive forensics. The blog mentions a tool called ![DriveFS Sleuth](https://github.com/AmgdGocha/DriveFS-Sleuth) to parse Google Drive File Stream disk artifacts and help find deleted files.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/8ebdc974-bd87-41e9-b745-22fc999b372b)

### Q6: What is time of the incoming connection on AnyDesk? And what is the ID of user from which the connection is requested? Format: verboten{YYYY-MM-DD-HH-MM-SS:user_id}
Flag: `verboten{2024-02-16-20-29-04:221436813}`

Reading the question, it mentions AnyDesk (a program for Windows that allows you to remotely access another computer). Analyzing Randon's machine, the AnyDesk directory can be found in `C:\Users\randon\AppData\Roaming\`. Doing some research online, I found this ![blog](https://medium.com/@tylerbrozek/anydesk-forensics-anydesk-log-analysis-b77ea37b90f1) that talks about AnyDesk forensics. The blog mentions that the information about successful AnyDesk connections are stored in `ad.trace` log file.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/e17436a1-2a60-4f29-b88c-dd807fbc69bc)

So by filtering 'incoming', the information about the AnyDesk connection can be found.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/8ff49551-44c5-4cc6-b8ea-60b45ba4750f)

### Q7: When was the shredder executed? Format: verboten{YYYY-MM-DD-HH-MM-SS}
Flag: `verboten{2024-02-16-08-31-06}`

Reading the question, it mentions a shredder being executed. I assume it means the `file_shredder.zip` file that was found in the previous question on Slack. However, it was already identified previously that the zip file was probably deleted since it was not present in the Downloads directory.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/8417b9d6-347c-44ce-bc43-3092be9bb2bf)

Looking at common Window artifacts, prefetch files should be the best bet to gather information on deleted executables. Using ![PECmd](https://github.com/EricZimmerman/PECmd) to parse the prefetch files, they can be further analyze using Timeline Explorer. Filtering with 'file_shredder', the `SourceModified` date of `BLANKANDSECURE_X64.EXE-DF0E2BF6.pf` can be identified. However, the date should be adjusted to 12H time format as mentioned by the authors.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/c5762d4a-a8dd-49de-98e3-cfd3fc130385)

### Q8: What are the answers of the backup questions for resetting the windows password? Format: verboten{answer_1:answer_2:answer_3}
Flag: `verboten{Stuart:FutureKidsSchool:Howard}`

The question mentions something about Windows password, so I assume the answer is in one of the Windows artifacts again. I randomly guessed that it was located in `SAM` hive since it stores local passwords for a Windows machine. Analyzing the artifact, the answers for the backup questions can be found in the `ResetData` entry.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/6c78005a-e239-4675-8cd4-92a73fe79fee)

### Q9: What is the single use code that he copied into the clipboard and when did he copy it? Format: verboten{single_use_code:YYYY-MM-DD-HH-MM-SS}
Flag: `verboten{830030:2024-02-16-23-24-43}`

The question mentions clipboard history, so I did some research online I found this ![blog](https://www.inversecos.com/2022/05/how-to-perform-clipboard-forensics.html). Pretty straightforward, just extract `ActivitesCache.db` and analyze it on DB Browser. The `ClipboardPayload` shows the clipboard content.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/c345777d-c3ea-48eb-98a8-8fd0d20adbab)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/adde87e8-c67f-487a-ac33-61b26ba03e0b)

For the epoch time, it should be adjusted to IST timezone format as mentioned by the authors. So we can identify the time and convert it to get the real time.
