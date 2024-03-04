## Task 1: kfc-with-who?
Question: "KFC with @‌Who?"
Of course you should know this meme as an average osu player - even ppy posted about it last year.
Check out where the legendary osu! KFC guy streams, and find the flag there.

Flag: `osu{mastery_in_terrible_rage-inducing_chokes}`

The question mentioned something about osu and KFC. Googling them together, we get this [reddit post](https://www.reddit.com/r/osugame/comments/15keb8j/everyone_is_having_kfc_with_thepoon_lol_d/) about osu players having KFC with a streamer called `ThePooN`. 

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/a33f0cb5-36f3-4adb-8df1-957fe1ad785e)

Checking his profile on osu! website, he linked his Twitch channel on his bio, making my search easier.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/ef33e760-81a8-4c1c-b12a-983228c07eac)

On his Twitch channel `About` page, the flag can be found.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/9b0aff86-95fb-43ce-bbc5-23ec113387b9)

## Task 2: when-you-see-it
Question: My friend is so obsessed with osu! that he refused to play any CTF! Today he came to me and sent me this weird GIF, can you understand what he is trying to tell me?

Flag: `osu{@nd_wh3n_y0u_fuxx1n_cL1ck3d_nd_c_1T!!!}`

We are given a gif file showing a person pointing at his monitor. Doing a simple reverse search, we find that the person was `Aireu`.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/69e5750f-650e-4bd8-b5b0-f7937d4bfe20)

Since we have no other information provided, I assume the gif contains important data. Using binwalk, a hidden zip file can be extracted.

```
└─$ binwalk -e challenge.gif 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             GIF image data, version "89a", 498 x 373
3410748       0x340B3C        Zip archive data, at least v1.0 to extract, name: secret/
3410813       0x340B7D        Zip archive data, encrypted at least v2.0 to extract, compressed size: 201, uncompressed size: 230, name: secret/confidential
3411107       0x340CA3        Zip archive data, encrypted at least v2.0 to extract, compressed size: 2821, uncompressed size: 317388, name: secret/secret.wav
3414272       0x341900        End of Zip archive, footer length: 22
```

However, the zip file was password-protected. Hence, I randomly tried `Aireu` as the password and it worked. The zip file contains a wav audio file and a text file which gives the first part of the flag and the hint for the second part.

```
HIGHLY CONFIDENTIAL

<REDACTED>
I have stored extremely important files here and in another place.

Find it at "osu_game_/[0-9]+/".

As a reward, here is the first part of the flag: `osu{@nd_`

Yours,

Team Osu!Gaming
</REDACTED>
```

Listening to the wav file, it seems to be a morse code. So using a [decoder](https://morsecode.world/international/decoder/audio-decoder-adaptive.html) online, the hidden message can be obtained.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/bed37de8-f52c-4541-95c6-340f574eb599)

I went ahead and further decoded the hidden message on CyberChef and found this number. Yes I play osu! back in the days and I know of the 727 meme. So I assumed `727727` might be required to find the second part of the flag.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/be3b130b-eb7a-4f53-acb7-9c1fecdfd30b)

Reading the text file hint again, it mentioned finding the second part of the flag at `osu_game_/[0-9]+/`. I assume this is a regex where the full string is `osu_game_727727`, at this point I was stuck, but my teammate @Zer01 found out it was a Twitter username all along. The display name is the second part of the flag.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/ed509e00-c4f3-4caf-a176-33fb570383a4)

Now to find the third part of the flag, the first link leads us to a Rick Roll ffs. However, going down his posts, we find something interesting related to his favourite beatmap.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/c7aaa962-9a2a-4a2b-9457-bd1cc58bf76f)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/66e93ffe-8497-4f88-b251-e0ead9c5d469)

Using the link, it leads us to his favourite beatmap (ClariS the goat). Clicking these dropboxes seems to be correct, so I just spam clicked all of them and the third part is obtained.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/36fdee5d-7227-4afc-9e27-8f6be3cecd4b)

## Task 3: when-you-see-it
Question: i (willwam) did a replay for a tournament quite recently (no more than 2 weeks ago) and hid a flag in it! can you find it?

Flag: `osu{m3ow}`

We are given a URL that redirects us to willwam's osu account. On the `tournament box` section, there are two hyperlinks `[a]` and `[staffing]`.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/4a88c569-9b7f-4c65-acac-32a678e7e298)

`[a]` holds willwam's tournament spreadsheets but there were nothing interesting in them. However, `[staffing]` holds his staffing history for previous tournaments.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/685d1a22-5337-47f2-9ddb-7195c4e26ccb)

Remembering that the question mentioned `no more than 2 weeks ago`, so it is probably this beatmap.

![osu](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/fb71d5b4-9f1a-4233-807c-fc199266752f)

Clicking on the `main sheet`, it leads to several social links. I went to his Twitch account and found several video archives.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/7892c9b8-f6eb-4946-952b-e3bd4ec48983)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/50c70897-8e71-4dc5-b288-85ea8f19ce5e)

Reading the challenge name again, it is related to `Mappool` so going through the VODs, the video can be found with the flag at the end.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/0f624b85-a79f-4b37-b17d-bc14b35fcf5d)

