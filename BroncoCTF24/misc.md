## Task 1: Countries Unite
Question: "yoshie" sent me a peculiar message. What could he possibly be trying to say?

Flag: `bronco{diveristyequityinclusion}`

We are given an image of Discord emojis. Just get the first letter of each country flag and the flag can be obtained.

![countries](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/9a509c5a-0fe8-4399-ab45-56c6a9e733b9)

## Task 2: BroncoCTF Crossword
Question: I am really annoyed. I work at Bronco Venture Accelerator and instead of doing work, my boss is just sitting doing a crossword. And drinking lemon juice? WHY! I want to dump it on him and his paper. We need to make MONEY.

Flag: `bronco{crosswords_do_not_increase_shareholder_value}`

We are given a pdf file of a crossword puzzle and it seems that there are white boxes covering the crossword. Initially, I spent an hour doing the crossword puzzle and it lead to no information on the flag.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/34e8cd87-6b66-4214-8b09-216284d2cfed)

Hence I had to ask hints from Discord which mentioned something about lemon juice and paper. This obviously means the text "disappeared" as lemon juice on ink produces invisible ink.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/ffdeacaf-488c-428d-8a79-38dd1b852343)

Sadly, I could not solve this challenge before the CTF ended, however, I found out that using a simple CTRL+A reveals the flag (bruh).

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/7c36d374-0d6f-4fc5-b4a1-1407d8a2710e)
