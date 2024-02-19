# Solution
## Task 1: Medieval Beats
Question: Check out my youtube video

Flag: `bronco{1n_17_f0r_7h3_10n6_h4ul}`

I could not solve this challenge before the CTF ended, however I tried it again the next day and finally understood the challenge. We are given a 1 hour Youtube video with characters of the flag coming up in random frames throughout, however, some of the frames were too quick for the human eye. One way is to watch the whole video in fast playbacks like 8x or 16x. Another way by @Krauq is to just download the video and extract each frames with a tool like [ffmpeg](https://ffmpeg.org/).

```
ffmpeg -i Flag\ Vido.mp4 -vf "fps=1" output_%04d.png  
find . -type f -size 284c -exec rm {} +
```

## Task 2: Wario Party
Question: Who is the true hero of the Mario Party games you might ask? Look inward and you might find it at the intersection of Mario's color and the number of brothers.

Flag: `bronco{b0ws3r_g0t_th4t_dumpy}`

We are given an image of Super Mario Bros poster that has a huge file size which suggest steganography. Reading the description carefully, it mentions something about Mario's color and the number of brothers.
```
Mario's color = Red
Number of brothers = 2
```

Using this information, it seems that a LSB steganography is performed. To ensure this logic is correct, analyzing the image shows that there data encoded in Red 2. So we can use online tools like [StegOnline](https://georgeom.net/StegOnline/upload) to extract the hidden data embedded within the image.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/4c1def34-1929-469b-80a3-22d97af2a94e)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/a5e3c450-eb85-4557-9fd9-0863c8def179)

The extracted data seems to be an image of another puzzle with Wario and Waluigi. Analyzing the image, it seems that it is another binary type puzzle where Wario=0 and Waluigi=1. Using CyberChef, the flag can be obtained.

<p align="center">
  <img src="https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/cddad5a1-3e3a-405a-92b5-c1dc19c0b1a9" alt="RealHeroOfMarioDecoded" width="30%" height="70%"/>
</p>

## Task 3: Boom
Question: With all these talks of arbitration, things are tense here around the office. I feel like people are going to explode at any moment. I gotta watch where I step before I accidentally bring something up and uncover something I didn't want to.

Hint: Look into Minesweeper and hex

Flag: `bronco{bo0m!}`

We are given a file with the .mbf extension, containing several hex digits. Initially having no clue on what the file should be used on, the authors provided a really good hint that helps clarifying this issue.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/3e390c5b-666e-4b14-9b43-ac23b70f0f13)

By Googling minesweeper and mbf, I come across [MZRG](https://mzrg.com/js/mine/make_board.html) that allows players to load custom minesweeper boards to a game using the hex values. After getting the minesweeper board, I had no clue what I was looking at. However, I noticed 'XD' was written using the mines at the right side of the board map. This suggest that the flag was probably the huge chunk at the left side of the board map.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/7f190d07-53af-4ff8-9faa-f97b7bba8802)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/a8210a79-1f35-4f4c-b598-169cfe944434)

## Task 4: Mystery Sound
Question: This transmission supposedly contains a secret flag, but I can't decode it because of some interference. Can you help?

Flag: `bronco{y0u_mu57_h4v3_4m4z1ng_h34r1ng}`

We are given a wav audio file in this challenge, which also means audio steganography. Using [Sonic Visualizer](https://www.sonicvisualiser.org/) on Windows, a spectrogram layer can be added and a weird encoded message can be found. It seems to either be digital waves or morse code. But after analyzing it, it was definitely digital waves where down = 0 and up = 1.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/a52eb414-979a-491b-b242-9713d6c44af0)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/df2db86b-0680-40f5-98c8-cd18e4405207)

So I manually decoded them using [cryptii](https://cryptii.com/pipes/binary-decoder) (had to repeat 6 times rip my eyes) and finally got the flag.

## Task 5: LAN Party
Question: My friend is SO MEAN! He changed my password on my home router and hid it in this Minecraft world. He even unmined the chunk I dug out...what a jerk. Ugh, now I am just here at the top of the world rather than at bedrock mining diamonds.

Flag: `bronco{b1rd's_Ey3_View}`

I could not solve this challenge before the CTF ended, however the authors gave me the method to solving it so I attempting it myself after. Since it was Minecraft forensics, I assume we had to analyze the chunk position in NBTExplorer to uncover hidden values. However, the authors mentioned that another specific tool can be used to get the flag directly.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/9e3162ce-ff7d-4930-afab-521dac860143)

As shown in the Discord chat, it seems that using [uNmINeD](https://unmined.net/) allows the player to see a top down view of a minecraft world, where the flag can be easily seen.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/70b616d9-a6bc-4462-a5ad-93c520823e17)
