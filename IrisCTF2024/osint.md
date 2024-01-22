# Solution
## Task 1: Czech Where?
Question: Iris visited this cool shop a while back, but forgot where it was! What street is it on?

![cw_1](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/8df8d357-7dec-4d4b-9232-d7e54a64607b)

Just perform a Google reverse search.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/40002440-6280-46ff-9ca3-24ebaccb64d5)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/851569f4-a4b3-492d-81f2-2f53fc3afb12)

Looking into Google Maps for Czech Golden Lane, we can find the street.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/cf929ac6-2d3b-4c40-b47d-6083647da853)

## Task 2: Away on Vacation
Question: Iris and her assistant are away on vacation. She left an audio message explaining how to get in touch with her assistant. See what you can learn about the assistant.

We are given a voicemail of a person named 'Iris' and the voice can be heard as:

```
Hello, you’ve reached Iris Stein, head of the HR department! I’m currently away on vacation, please contact my assistant Michel. You can reach out to him at michelangelocorning0490@gmail.com. Have a good day and take care.
```

Funny thing is I actually went to email the person since the admin told me its allowed and I actually got an automatic reply from Michel stating that he is also on vacation now and I can reach out to him via social media.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/4a8573dd-93be-4a77-8ff5-3b400035a1ae)

So I tried finding him on Instagram and one of the post gave the flag.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/bc0e1b75-2f78-4f0a-8899-e8e9d15c5dcb)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/002120aa-b0c9-454e-af2f-185b950d6989)

## Task 3: Personal Breach
Question: Security questions can be solved by reconnaissance. The weakest link in security could be the people around you.

In this challenge, we are tasked to look for information about Iris Stein:

1. How old is Iris? 
2. What hospital was Iris born in?
3. What company does Iris work for?

Going through Michel's followers, we find Iris Stein. Going through all her posts, we find that even her mom, Eleina Stein, has a social media account LOL.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/17bbbb5b-8c2c-439a-a4e6-f174091d9f8c)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/8856ca3f-2b72-41c2-beba-965acccd17db)

Looking everywhere for her mom, the only place I got information was Facebook (classic boomer).

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/bd399e59-fa54-4584-8fb0-af376e6ee412)

Going through Eleina's posts, we find a life event post on her daughter's birth. There we can find Iris's age.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/221f129b-9a81-480e-b2be-b284d408f9c8)

To find the specific hospital, just Google reverse search the hospital picture posted by Eleina.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/38d2ebd4-82c9-48e7-a1e8-0fbac59c2bc3)

Our last task was to find the Iris's company. These work-related stuff should always be found via Linkedin. Remember to type in HR for easier search since we know she is the HR department from the voicemail in Task 2.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/e14588f6-0ed2-4161-a860-050b74c6837c)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/c907c927-c9ff-4f22-a256-3a8342abd770)

And boom we get the flag after answering all the questions.

![pb_8](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/9b00deec-1fa6-4a85-beac-5732df43a8d9)

## Task 4: A Harsh Reality of Passwords
Question: Recently, Iris’s company had a breach. Her password’s hash has been exposed. This challenge is focused on understanding Iris as a person. Hash: $2b$04$DkQOnBXHNLw2cnsmSEdM0uyN3NHLUb9I5IIUF3akpLwoy7dlhgyEC

I was not able to solve this question before the CTF ended, however, I did attempt it later on and found it quite interesting. 
Basically from what the scenario mentioned, we have to find several characters and digits to create our own password dictionary to crack the hash given. Reading the hash, it is probably a `bcrypt $2*$, Blowfish (Unix)`

We can see that she calls her Mothers birthday a 'very important date' so thats one of the details we can extract for our dictionary.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/7f97f295-5816-4d78-a549-21bbc59f0ef8)

Here, she mentions her love for Tiramisu, that's going on the dictionary.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/c296e532-09d2-49ce-9711-165e22b97b1f)

In this post she talks about an important place in Italy, Portofino.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/3025c3fd-ff9c-4090-9731-a36e560f6614)

Using this simple Python script, we created our own dictionary with several words and her mother's birthdate (the admins gave us a hint on this).

```
from itertools import permutations

words = ["Iris", "Stein", "Elaine", "Tiramisu", "Portofino", "Mimosas", "Italy"]
word_combinations = permutations(words, 3)
all_word_combinations = [''.join(combo) for combo in word_combinations]

date_numbers = "8041965"
num_combinations = permutations(date_numbers, len(date_numbers))
all_num_combinations = [''.join(combo) for combo in num_combinations]

combined_results = [word_combo + num_combo for word_combo in all_word_combinations for num_combo in all_num_combinations]

# Save the output to a text file
output_file_path = "output.txt"
with open(output_file_path, 'w') as output_file:
    for result in combined_results:
        output_file.write(result + '\n')

print(f"Output saved to {output_file_path}")
```

Using hashcat, we can crack the hash and obtain the password.

```
hashcat -m 3200 hash output.txt
```
