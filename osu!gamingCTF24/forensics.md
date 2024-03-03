## Task 1: nathan-on-osu
Question: Here's an old screenshot of chat logs between sahuang and Nathan on hollow's Windows machine, but a crucial part of the conversation seems to be cropped out... Can you help to recover the flag from the future?

Flag: ``

The question provided us a cropped png image. This is a classic forensics challenge in CTFs and by using this [tool](https://acropalypse.app/), the flag can be obtained with the right resoulation.

## Task 2: volatile-map
Question: Hey osu! players, our SOC team was informed that a group of spies from Mai Corp is trying to sabotage our infrastructure via their secret map in osu!.
We were able to break into their rendezvous, but they noticed we were stealing their data and they corrupted them in time. Fortunately, we managed to acquire a full memory dump from one of their machines.
Can you help us investigate what they were trying to do?

Flag: `osu{hide_n_seeeeeeeeeek}`

The question provided us a memory dump (my favourite). So by using volatility3, I managed to find a suspicious commands from notepad using cmdline plugin.

```
7928    notepad.exe     "C:\Windows\System32\notepad.exe" C:\Users\Administrator\AppData\Local\osu!\Songs\beatmap-638448119315467561-ambient-relaxing-music-for-you-15969\osu{ - 686964655f6e (sahuang) [X3NlZWVlZWVlZWVla30=].osu
```

Analyzing this command closely, it seems that the flag is basically the command line. where `686964655f6e` is hex encoded and `X3NlZWVlZWVlZWVla30=` is based 64 encoded. After decoding them, the flag can be obtained. PS: I got 3rd blood for the first time!!

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/00059c0b-c825-45bd-945a-3352bf4cd343)
