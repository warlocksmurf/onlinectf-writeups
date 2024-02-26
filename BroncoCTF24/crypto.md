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

We are given a pdf with many 6-band resistors. After awhile, I stumbled upon this [tool](https://www.geocachingtoolbox.com/index.php?lang=en&page=resistorCode) that helps calculate the Ω values for each resistor.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/cd7dade1-ca4a-43f1-b13c-8a935d1c7c23)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/fa533ead-7154-414b-9340-de2b6731104c)

Noticing that the Ω values seems to represent certain numbers, I tried every resistor's Ω value on CyberChef and it succesfully decoded the flag. Thanks @eror__404 for the sanity check.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/d28d9846-1d73-4eec-9a20-0858d0c53b51)

## Task 4: Oh, Danny
Question: When using AES in CBC mode, Danny has a habit of leaving messages in his initialization vectors. Can you find his secret message?
```
key = 73757065725f6b65795f73747265616d
pt1 = 4163636f7264696e6720746f20616c6c
pt2 = 206b6e6f776e206c617773206f662061
ct2 = 817ed4df4521cc2d6e746c45a834aa2d
```

Flag: `bronco{d0nt_l3@k_ur_k3y}`

From the description, it seems that we have to reverse the AES-CBC encryption to obtain the IV. I could not solve this challenge before the CTF ended, however I tried it again the next day since I knew the concept.

![cbc](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/cdb052c7-fd53-49f9-8ea7-31675b1f7b0d)

So I created a simple Python script to reverse the encryption process and obtain the IV. This script was heavily inspired from @Krauq writeup so credits to him.

```
from Crypto.Cipher import AES
from binascii import unhexlify

key = unhexlify("73757065725f6b65795f73747265616d")
ct2 = unhexlify("817ed4df4521cc2d6e746c45a834aa2d")
pt2 = unhexlify("206b6e6f776e206c617773206f662061")
pt1 = unhexlify("4163636f7264696e6720746f20616c6c")

# Decrypt ct2 to get "pt2 XOR ct1"
cipher = AES.new(key, AES.MODE_ECB)
decrypted_ct2 = cipher.decrypt(ct2)

# Calculate ct1
ct1 = bytes(a ^ b for a, b in zip(decrypted_ct2, pt2))

# Decrypt ct1 to get "pt1 XOR IV"
decrypted_ct1 = cipher.decrypt(ct1)

# Calculate IV
iv = bytes(a ^ b for a, b in zip(decrypted_ct1, pt1))
print(iv)
```

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/851c6886-3a8c-4152-b95f-98d5f246571d)
