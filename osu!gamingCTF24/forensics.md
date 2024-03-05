## Task 1: nathan-on-osu
Question: Here's an old screenshot of chat logs between sahuang and Nathan on hollow's Windows machine, but a crucial part of the conversation seems to be cropped out... Can you help to recover the flag from the future?

Flag: `osu{cr0pp3d_Future_Candy<3}`

We are given a cropped png image. For this challenge, I could not solve it before the CTF ended, but I knew it was a common forensics challenge in CTFs where we can use tools such as [Acropalypse](https://acropalypse.app/) to recover the image with the right resolution.

![nathan_on_osu](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/aeebc31f-0bb4-4ffc-bf8d-47ef9caf83b5)

However, the web tool did not work for me for some reason. So I went ahead and used [Acropalypse-Multi-Tool](https://github.com/frankthetank-music/Acropalypse-Multi-Tool/tree/main) instead. PS: when downloading the tool and receiving this error, just change the code to `from gif_lib import *`.

```
PS C:\Users\ooiro\Desktop\Acropalypse-Multi-Tool-1.0.0> python gui.py
Traceback (most recent call last):
  File "C:\Users\ooiro\Desktop\Acropalypse-Multi-Tool-1.0.0\gui.py", line 15, in <module>
    from gif_lib import acropalypse_gif
```

Running the tool with the appropriate width and height, the flag can be obtained.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/9a282e4d-061b-43be-bb8a-0a71f5288710)

![nathan flag](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/4035976e-7320-4ecb-8557-7182a2dea9aa)

## Task 2: volatile-map
Question: Hey osu! players, our SOC team was informed that a group of spies from Mai Corp is trying to sabotage our infrastructure via their secret map in osu!.
We were able to break into their rendezvous, but they noticed we were stealing their data and they corrupted them in time. Fortunately, we managed to acquire a full memory dump from one of their machines.
Can you help us investigate what they were trying to do?

Flag: `osu{hide_n_seeeeeeeeeek}`

We are given a memory dump (my favourite). So by using volatility3, I managed to find `osu!.exe` program running `notepad.exe` instances for some reason (kinda sus).

```
└─$ python3 vol.py -f ~/Desktop/sharedfolder/osu/memory.dmp windows.pstree 
Volatility 3 Framework 2.5.2
Progress:  100.00               PDB scanning finished                                
PID     PPID    ImageFileName   Offset(V)       Threads Handles SessionId       Wow64   CreateTime      ExitTime

...
*** 2272        4556    osu!.exe        0xca8603f1f080  34      -       1       True    2024-03-01 13:41:15.000000      N/A
**** 7928       2272    notepad.exe     0xca8604d4d340  3       -       1       True    2024-03-01 13:42:17.000000      N/A
**** 6188       2272    notepad.exe     0xca8604d77340  3       -       1       True    2024-03-01 13:42:38.000000      N/A
```

So I used the cmdline plugin to check for suspicious commands and I was right, notepad has a command that seems to be the flag itself.

```
└─$ python3 vol.py -f ~/Desktop/sharedfolder/osu/memory.dmp windows.cmdline
...
7928    notepad.exe     "C:\Windows\System32\notepad.exe" C:\Users\Administrator\AppData\Local\osu!\Songs\beatmap-638448119315467561-ambient-relaxing-music-for-you-15969\osu{ - 686964655f6e (sahuang) [X3NlZWVlZWVlZWVla30=].osu
```

Analyzing this command closely, it seems that the flag is basically the command line. where `686964655f6e` is hex encoded and `X3NlZWVlZWVlZWVla30=` is based 64 encoded. After decoding them, the flag can be obtained. PS: I got 3rd blood for the first time!!

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/00059c0b-c825-45bd-945a-3352bf4cd343)

## Task 3: out-of-click
Question: I love playing this map but recently I noticed that some of the circles seem off. Can you help me find the locations of the weird circles?

Flag: `osu{BTMC_15_mY_G0aT}`

We are given a beatmap folder. Looking into it, there are different types of the beatmap (Normal, Out of Click, Time Freeze). Reading the question, I assume that the differences can be found by comparing `Normal` and `Out of Click` of the beatmap. 

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/f1d5e5d1-04e0-43ed-add6-1cc43738504d)

Using this online [tool](https://www.diffchecker.com/text-compare/), we can compare the differences of `[HitObjects]` between the two modes of the beatmap. The tool shows 5 additional lines placed within the `Out of Click` mode.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/1f1bee6f-e851-4594-b01b-332d54d809bc)

Randomly placing the first four digits of each line on CyberChef, it translates to the flag.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/5924eab3-3538-4d47-b63d-af8a1c8a249b)
