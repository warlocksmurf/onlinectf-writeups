## Task 1: ACM Borg Members
Question: I am convinced the board members of Santa Clara's ACM clubs are cyborgs! They are definitely digitally enhanced! ACM Board? More like, ACM-BORG! If only I had a way of proving it.

Flag: `bronco{be3p_b0op_@CM_are_cyb0rgs}`

Reading the title and description, it seems that I should be using robots.txt on a specific website.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/6a553df0-6cbf-4990-a841-47dbe8cd02c5)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/a419201e-f7bc-4e02-9e9d-aa9aa2b78e84)

## Task 2: Blue Boy Storage
Question: This blue boy saved something on his home planet but cannot seem to find it. Can you help him?

Flag: `broncoctf{ab4_d3_4ba_d1e_1m_blu3}`

We are given the [website](https://blue.web.broncoctf.xyz/), and what I always do is to look at the source code for any open flags.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/cf66e6e6-81d3-4274-8d90-95aaa539649a)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/c47a41a6-72c4-4dfe-8b4d-f1dcc1ef40a6)

Analyzing the javascript, the flag can be found as expected.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/4879685b-42a4-4b72-bdd5-1779fff38a57)

## Task 3: All I Do Is
Question: I LOVE TO ROLE PLAY! for my upcoming convention, i am reliving my glory days of being a minecrafter.

Flag: `bronco{Finding_diamonds_aint_so_hard_just_dig_baby_dig}`

We are given a [Youtube video](https://www.youtube.com/watch?v=DLgYt-569jc) and a [website](https://diamonds.broncoctf.xyz/), however the website seems to not work.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/2f7b0b6b-1f2d-43c6-ab02-ca8ad5ebfa11)

Watching the video, it was a Minecraft song on digging. Noticing how the name of the challenge closely resembles the video title, I assume we have to use the dig command, specifically to dig any TXT info in the URL.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/dc4c5d0f-78b8-4e8d-9880-0be13854f0ea)

## Task 4: Blue Herring
Question: This page contains the elusive blue herring, however it's never been seen by the human eye. See if you can catch it and rip it open to find a flag.

Flag: `broncoctf{D1s_H3rr1ng_Sh0uld4_B33n_Blue}`

We are back at Task 2 [website](https://blue.web.broncoctf.xyz/) again and this time there is another hidden flag inside it. By grepping the word "herring", a path to an image can be obtained.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/fcc73a62-9f01-4150-8882-2fb1da4af253)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/fb5e55fb-94e1-4748-b6b6-1998a3eaae5f)

At this point I had no clue what to do next, but after asking for hints, it seems I have to perform steganography on it (this should be a forensic challenge, not web). Since the question mentioned Blue herring, I thought of LSB steganography again. Hence I went ahead and tried zsteg and the flag can be located on Blue 8 LSB.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/a99abbf8-8ef8-44a6-abc2-309c48ef2df0)

However, the intended method was to just select every Blue bit MSB on [StegOnline](https://georgeom.net/StegOnline/extract) and the flag can be obtained.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/9c755f4f-1f5d-4cf8-a32f-ca7aae2d2dee)
