## Task 1: The end is beginning
Question: Me and my friends just finished our final semester of B.Tech, so we decided to have a trip somewhere, but due to some reason, many of them were not available for the trip, but we were all ok as less is more. As the trip was about to end, one of my friends said we should try scuba diving here. I was scared of that, but my friends said, If you don't risk anything, you risk everything. Seriously, why do we have to risk our lives for half an hour? It's impossible for me, I said. But they motivated me all night, and then it was time for the dive. I screamed, Impossible is not a word in my vocabulary, and dived in. After all this, when I came back to my room, I realised I was low on money, so I called and asked my father for some help by singing something like this:

```
I’d be gone to my dad
And ask for some cash
I ran ......
```
All the Hustle towards the trip was worth it, as we enjoyed it a lot and made some awesome memories throughout the trip. Flag format: VishwaCTF{My Name according to story_Amount I got in figures}

Flag: `VishwaCTF{Paradox_5000}`

Search the lyrics and it leads to this song with the singer's name.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/52614084-fe1c-494b-a7d4-5aa68c54ddb1)

Near the starting part of the lyrics, it seems that the rapper is saying something about his father giving money.
```
Hazaae mein puri aish
Hello papa 5k kar do UPI
Hain? Kya? Kis liye? Why?
```

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/8c8bdf8e-a7d9-4df1-9774-77d811a20f5f)


## Task 2: TRY HACK ME
Question: TryHackMe is a browser-based cyber security training platform, with learning content covering all skill levels from the complete beginner to the seasoned hacker.
One of our team member is very active on the platform. Recently, I got to know that he comes under 3% in the global leaderboard. Impressive isn't it.
Maybe you should have a look at his profile
PS : He keeps his digital identity very simple. No fancy usernames. It's just a simple mathematics
His real name == His username

Flag: `VishwaCTF{Pr0f1l3_1dent1fi3d_v0uch3r5_cr3d1t3d_5ucc355fully}`

The question mentions that one of the staff of VishwaCTF is top 3% globally on TryHackMe. So I went ahead and analyzed the staff and authors of VishwaCTF on their main website.
 
![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/dbdcc8d1-f140-4cda-a2f9-dca947e9fa58)

Looking at their members on the CTF website, it seems that `Ankush Kaudi` is the right person. Using his name on TryHackMe, the profile can be found with the flag.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/dabbaac1-c4b0-492f-b130-a62cf13f3f82)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/4897677b-22cc-4b1b-aa65-e341f3df9c57)

## Task 3: ifconfig_inet
Question: In the labyrinth of binary shadows, Elliot finds himself standing at the crossroads of justice and chaos. Mr. Robot, the enigmatic leader of the clandestine hacktivist group, has just unleashed a digital storm upon Evil Corp's fortress. The chaos is palpable, but this is just the beginning.
As the digital tempest rages, Elliot receives a cryptic message from Mr. Robot. "To bring down Evil Corp, we must cast the shadows of guilt upon Terry Colby," the message echoes in the encrypted channels. However, in the haze of hacktivism, Elliot loses the crucial IP address and the elusive name of the DAT file, leaving him in a digital conundrum.
To navigate this cybernetic maze, Elliot must embark on a quest through the binary underbelly of Evil Corp's servers. The servers, guarded by firewalls and encrypted gatekeepers, conceal the secrets needed to ensure Terry Colby's fall.
Guide Elliot to the his destiny. Flag Format : VishwaCTF{name of DAT file with extension_IP address of Terry Colby}

Flag: `VishwaCTF{fsociety00.dat_218.108.149.373}`

Reading the question, it is very obvious that I have to perform OSINT on the movie Mr. Robot to find the IP and the file name. Doing some quick Googling, the flag can be obtained.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/ebeddafa-baf1-4e12-8544-18e51e7b358e)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/07b7a24f-9a2d-43d5-967b-c95995507429)
