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

## Task 4: Sagar Sangram
Question: Once upon a time there were 2 groups, one was known for being on positive side and other on negative side. Both the groups wanted to be immortal and hence required a divine potion for it. So they decided to "Churn the Ocean" to get the divine potion. It's a wonderful event written in some of the ancient scriptures. Hop on to the server and have a chat with bot, maybe he'll give you the flag.

Flag: `VishwaCTF{karmany-evadhikaras te ma phaleshu kadachana ma karma-phala-hetur bhur ma te sango stvakarmani}`

We are given a Discord server link where the bot is located in. Answer a few questions from the bot and the flag will be given (use ChatGPT).

The questions:
```
Q1 of 10 : So it was decided, to obtain the divine potion of immortality, churning of the ocean is to be performed. For that purpose a huge mountain was used. Tell me what is the name of the mountain and also which ocean was churned to obtain the potion of immortality?
Ans fromat : name of ocean without space_name of mountain

Q2 of 10 : Let's get step back. Mount Mandara was used in churning. But it was not just around the ocean. It was brought there by someone. So, who brought Mount Mandara to Kshirasagara?

Q3 of 10 : Now the stage is set but to churn the ocean, something was required which both the groups would hold and churn. Who was used like a rope to churn the ocean Kshirasagara?

Q4 of 10 : The process starts and the outcomes begin to appear. One such outcome was a very threatening substance, which had the power to destroy the whole universe. But 'The Ultimate Destroyer' comes to rescue and consumed it, which results in his throat turning blue hence he is also called 'Neelakantha'. What is the substance called?

Q5 of 10 : Let's talk about few more outcomes. One such divine outcome was a tree. It was taken to the abode of Indra in swarga. It is often referred to as 'Wish Fulfilling Tree' as it  possess the power to bring one's imagination into reality. Tell me the name of this tree?

Q6 of 10 : Another creature appeared was a very powerful elephant which was taken by Lord Indra as his medium of transportation. It was very powerful elephant and also referred sometimes as 'King of Elephants'. What is the name of that elephant?

Q7 of 10 : After a while during the process, a bow appeared during the churning. It was given to Lord Vishnu as a weapon. What is the name of that divine bow?

Q8 of 10 : In ancient times as mentioned in the scriptures, conch was used as a sign to initiate a war between two groups (also used for other purposes as well). Different persons from both the sides would blow the conch which will mark the start of the war. During the churning, one such conch was obtained and it was given to Lord Vishnu. It's sound symbolizes the 'Sound of Creation'. What is the name of the conch?

Q9 of 10 : The fortunes turned as the goddess of fortune herself appeared. Every wanted the goddess of fortune to be at their side, but the destiny has it's own plan. She chose Lord Vishnu as her eternal consort.. Who is the goddess of fortune?

Q10 of 10 : Ok, let's end this thing. After all the struggle from both the sides, the long wait comes to an end. The divine potion is here and it is brought by none other than the physician of the devas. He is also referred to as 'God of Ayurveda'. Tell me his name and also the name of divine potion?
Ans format : name of the physician_name of divine potion

Impressive. A perfect 10/10. You are one the who deserves the flag. Just one last thing. All the event which is I asked you about is very popular and is mentioned in various scriptures like Vishnu Purana, Mahabharata, etc. Can you tell me what this event is popularly known as?
Use _ in place of any space
```

The answers:
```
Kshirasagara_Mandara
Garuda
Vasuki
halahala
Kalpavriksha
Airavata
Sharanga
Panchajanya
Lakshmi
Dhanvantari_Amrita
Samudra_Manthana

Perfect. That's all about this challenge. Hope you enjoyed it.
Thank you for playing VishwaCTF'24. Here you go with the flag for the challenge 'Sagar Sangram'
VishwaCTF{karmany-evadhikaras te ma phaleshu kadachana ma karma-phala-hetur bhur ma te sango stvakarmani}
```

## Task 4: Cyber Pursuit Manhunt
Question: In response to alarming reports, our cybersecurity team is actively pursuing a hacker known by the alias "h3ck3r_h3_bh41", who poses a serious threat by extorting innocent individuals for monetary gain. Your mission is to track down this hacker and provide us with the crucial information needed to apprehend them. Retrieve the Hacker's complete full name (first name, middle name, last name), formatted in lowercase and replacing spaces with underscores, along with the associated website domain. Flag Format: VishwaCTF{full_name_domain.in}

Flag: `VishwaCTF{}`

We are given a handle of the hacker, so I used it on Twitter/X and it gives the hacker's account. However, his posts seem to be unrelated to the challenge, hence I looked into his followers. Thinking I was in the right track, I asked the admins if the followers are related to the challenge, but its not (rip my 2 hours). So I had no clue how to complete it.
