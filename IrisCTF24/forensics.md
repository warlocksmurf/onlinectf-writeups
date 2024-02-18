# Solution
## Task 1: Not Just Media
Question: I downloaded a video from the internet, but I think I got the wrong subtitles. Note: The flag is all lowercase.

Flag: `irisctf{mkvm3rg3_my_b3l0v3d}`

We are given a weird .mkv video file and reading the scenario, we have to probably analyze the subtitles. So doing my research online, we can use [mkvinfo](https://linux.die.net/man/1/mkvinfo) to find multiple tracks and subtitles embedded within the video.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/73facf85-395c-4be4-bafa-d7344fc85824)

Analyzing the metadata, we notice 1 of the font files called FakeFont.ttf (kinda suspicious). So we extract this font file and also the subtitle of the video.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/6d7bad50-4c28-41d4-b3a6-a28e041b226a)

In the subtitles file, we can find the subtitles being several chinese letters. Since we already have the font file, we just have to combine them together using an online tool called [fontdrop.io](https://fontdrop.info/#/?darkmode=true).

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/97222741-0790-40ca-ab72-29fdc153e6fc)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/edc43207-90cc-4854-9dd1-cb907a6e2b3f)

## Task 2: skat's SD card
Question: "Do I love being manager? I love my kids. I love real estate. I love ceramics. I love chocolate. I love computers. I love trains."

Flag: `irisctf{0h_cr4p_ive_left_my_k3ys_out_4nd_ab0ut}`

We are given a Linux file system, so I opened up Autopsy and started searching for clues. The first thing I always check is the user, and analyzing the hidden files in the user skat, we can find that skat probably downloaded something from GitHub. 

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/84d885e6-6a59-4da9-9b23-ea8432be1a3d)

So I attempted to clone it myself but it requires a secret key. At this point I could not solve it before the CTF ended, but I asked several members on Discord and they told me we can actually extract the public key from the hidden files. Going through the directories, we can find another hidden directory called .ssh and within it the RSA keys.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/afd61e8d-2fcd-41a3-9eec-c70a496c7e07)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/3ba1a053-9184-4a76-918e-81fb6234ab85)

Since its a private key, we must brute force it using John.

```
ssh2john id_rsa > id_rsa.hash
john --wordlist=$rockyou id_rsa.hash
```

The password  is 'password' (that's not very secure skat!!). With the password, we can start cloning the suspicious files. Before that, we have to also copy both the private key and public key to my `~/.ssh` directory to be used for authorisation. After cloning the repository, we can find numerous text files, README, and a hidden .git directory.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/2409371d-4c90-42c0-b7ae-23382d8a6aac)

@seal on Discord told me that we can use a tool called [packfile_reader](https://github.com/robisonsantos/packfile_reader) to extract and parse .git data to some text files. Navigating to `.git/objects/pack`, we can utilize the tool and just grep the flag from the parsed text files.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/184b0aba-837d-4bba-9cb7-71a43a29e691)
