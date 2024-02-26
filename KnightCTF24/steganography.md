## Task 1: Oceanic
Question: The ocean's beauty is in its clear waters, but its strength lies in its dark depths.

We are given an image and a .wav audio file. So I tried analyzing the audio file on Audacity but nothing relevant was found.

So I exiftool the image file to find an encoded message in it. It is probably a hint to find the flag, so I did some research on "deepaudio" and came across a 
[blog](https://medium.com/@ibnshehu/deepsound-audio-steganography-tool-f7ca0a897576) on audio steganography with the tool called `DeepSound`, kinda sus as it represents the challenge and the hint given.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/945737b9-c2ab-46ae-bedf-cdfc19851326)

Using the password obtained in exiftool, we can extract a `flag.png` from it. So we binwalk the png and find the flag.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/d7142a6e-24df-422f-923c-0a5acee4d089)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/5bcb67ad-fc61-4791-99f3-d2a5f868b974)

## Task 2: Flag Hunt!
Question: Hunt your way through the challenge and Capture The hidden Flag!!!

We are given a protected Zip file. Using binwalk, we can find multiple jpg files, text files and a wav file. So, since I have no clue what the password is, I just brute-force it with John.

```
zip2john chall.zip > hash
john --wordlist=$rockyou hash
```

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/fb066850-f28c-4a45-9e97-e78af9809821)

After extracting the zip with the password, we can start analyzing the text files. I used `files *` to analyze all files and found out img725.jpg is different from the rest.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/e146ba71-4dc2-478d-bad6-633dbe5b4d50)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/e6bfc8f3-2d6d-4996-a42f-20aacf8acc1c)

Listening to .wav audio file, it is a Morse code. Tried the code obtained in Morse code as the flag, sadly it was incorrect. 
So I tried stegsolve on the outlier image file and got the real flag.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/52846ce6-c9b7-4d17-a630-6da3bc83b8c5)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/b8396e6f-46fe-4343-9100-80798914518d)
