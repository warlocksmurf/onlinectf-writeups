## Task 1: sanity-check-1
Question: My first map in 2024: Gawr Gura - Kyoufuu All Back
Mapped for osu!gaming CTF 2024. Please play/watch the top diff with video on!
To osu! players: CTF is a competition where participants attempt to find text strings, called "flags", which are secretly hidden in purposefully-vulnerable programs or websites. The strings can be in plain text, images, encoded/encrypted, etc.
In sanity-check-1, checkout map metadata and submit the flag you found. Remember flag is always in format osu{...} unless otherwise specified.

Flag: `osu{welc0me_2_osu!!}`

We are given a link to a beatmap called `Gawr Gura - Kyoufuu All Back`. The flag is there.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/5132a901-afdb-437f-a0f9-eba642512920)

## Task 2: smoked
Question: Imagine getting flag from simply watching a replay...

Flag: `osu{smoked_map_fun}`

We are given a osu! replay file (.osr). Just enjoy the [gameplay](https://link.issou.best/PK1Tr3) and the flag can be seen in the replay.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/9cc11079-5ac7-4559-b1b7-ff576af6ac9d)

## Task 3: multi
Question: Multi is a game mode in osu! for people to play together in a multiplayer lobby. PvP is fun, isn't it?
Join the official CTF lobby osu!gaming CTF 2024 to claim the flag! You are more than welcomed to stay in the lobby for a bit longer and have fun with others. There may or may not be secret prizes for random game winners :D Note: Please behave nicely and follow the rules. Avoid changing lobby name, kicking players, or spamming in chats. Do not spoil the fun for others! Report any misbehaviour to admins.

Flag: `osu{Ur_welcomed_to_play_a_few_games_in_MP_lobby<3}`

Follow the question, join the Multi lobby and enter `!flag` (had to install back osu for this lol).

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/9106b9d7-7ccb-40ba-ad4a-4f5757aae8fd)

## Task 4: osu!crossword
Question: Come solve my osu!crossword challenge! Once you completed the puzzle and obtained the hash, submit it to remote server for verification. Note: All characters you enter in crossword should be in lowercase.

Flag: `osu{Much_34s13r_Th4n_osu!Trivium_XD}`

Just play the crossword puzzle. PS: ngl the crossword website they made was pretty cool.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/0f1499fe-15e1-464b-8b01-fe637f058e8f)

```
└─$ nc chal.osugaming.lol 7270
Submit the hash: b85478a2d0c66c43f395ab166a6a4aa07a39fdb08e097d23f1d057746887d37a
Congrats! Here's your flag: osu{Much_34s13r_Th4n_osu!Trivium_XD}
```
