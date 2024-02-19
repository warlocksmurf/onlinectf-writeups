# Solution
## Task 1: Preschool Lessons
Question: a b c... easy as 1 2 3... Do you REALLY know your ABCs? 
```
abbaaabacabbbaabacabbabbbbcabbabbbacabbaaabbcabbabbbbcabbbbabbcabbabaabcababbbbbcabbabbabcaabbaaabcabbbaabbcabbbaabbcababbbbbcabbbaaaacabbbaabacaabbaabbcabbbaabbcabbaaabbcabbabaaacabbabbbbcaabbaaaacabbabbaacabbbbbab
```

Flag: `bronco{i_m1ss_pr3scho0l}`

Reading the description, it is obvious that abc maps to 123. But thinking about it, it does not map to anything related to an encoding method or the flag. After analyzing the text for a long time, I noticed that it looks like binary again (the authors love binary for some reason) where c represents the space between a byte. So I mapped a=0 and b=1 and it worked.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/d0e13950-97df-4bb0-80a5-52e5c0a0617e)

## Task 2: Zodiac Killer
Question: The Zodiac Killer is on the loose! I saw this message spray painted on a wall.

Flag: `bronco{LOOKOVERYOURSHOULDER}`

<img width="473" alt="zodiacchal" src="https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/cea38f9b-bf35-453d-beb5-315b99aa72be">

The image shows a simple Zodiac Killer Cipher, just decode it.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/9562f062-32fd-4e39-b079-556bee9ea939)

## Task 3: Electrical Engineering
Question: I hate electrical engineering

Flag: `bronco{rEsi5t_ev1L}`

We are given a pdf with many 6-band resistors. After awhile, I stumbled upon this [tool](https://www.geocachingtoolbox.com/index.php?lang=en&page=resistorCode) that can help me comverting the color code. Going through each column, we can get the flag by converting the Ω values to Ascii.

