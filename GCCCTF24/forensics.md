## Task 1: BipBipBiiip
Question: Introduction to anomaly detection. Find the phone numbers that are not formed in the correct way and decode the hidden message.

Flag: `GCC{R3g3x_4r3_W1ld!!!!}`

The question provided us a csv file of a phonebook. Looking at the entries, the phone numbers were suspicious as they were weirdly placed like hex or some sorts. So regex knowledge was required in this challenge (which me and teammate suck at).

```
└─$ cat phonebook.csv | head
ID,FIRST_NAME,LAST_NAME,MAIL,PHONE_NUMBER,ADDRESS
b35cd960-86ba-4697-a2f8-4eecd50b77e8,Margaret,Perrin,aliceblot@example.com,001-936-209-2959x28564,"11911 Rachel Point South Tamarahaven, VA 11658"
a24dfa82-4d8e-4278-a6a6-d96af4c50c96,Jeannine,Roman,yui46@example.org,090-8945-0526,群馬県山武郡芝山町美原町11丁目15番3号 パレス花川戸304
dec0464c-ca08-4a38-ac4e-22f404ea2711,亮介,Baudry,mcclainamy@example.com,070-0593-0250,"USCGC Evans FPO AP 68744"
8b0db152-34a0-4016-b8f1-99e5358bff06,Michael,De Oliveira,agathenguyen@example.org,+33 7 90 46 14 42,"967 Timothy Mews Suite 851 Sarahstad, FL 27297"
88902ce2-db03-4c65-8574-0739276165bd,直人,Lewis,bourgeoisvirginie@example.org,+33 (0)5 49 87 88 43,宮崎県印西市大中28丁目18番11号
206f7df8-8193-4cdd-a4a2-ab958d50fd90,直子,池田,trananthony@example.com,03-1255-1140,"boulevard Vallée
```

Our unintended method was pretty funny, what we did was guessing the flag by changing the starting parts of the flag `GCC{` to hex, and using the hex to find each number. The first step was to extract phone numbers only from the csv file. This can be done with a simple script @Odin did.

```
import csv
with open(./phonebook.csv', mode='r', encoding="utf8") as file:
    csvFile = csv.reader(file)
    for lines in csvFile:
        with open("output.txt", "a") as file_written:
            file_written.write(lines[4])
            file_written.write("\n")
```

Now we have to perform our filtering to find anomalies and we found this [StackOverflow post](https://stackoverflow.com/questions/13719367/what-is-the-best-regular-expression-for-phone-numbers) that talks about the regex for phone numbers. Creating another simple script to filter out patterns that do not correlate to phone numbers.

```
import re

def validNumber(phone_number):
    pattern = re.compile("^[\dA-Z]{3}-[\dA-Z]{3}-[\dA-Z]{4}$", re.IGNORECASE)
    return pattern.match(phone_number) is not None
with open("output.txt", "r") as file:
    for i in file.read().split("\n"):
        if validNumber(i) == False:
            with open("not_correct.txt", "a") as file_written:
                file_written.write(i)
                file_written.write("\n")
```

After that we perform our guessing game LOL. Since we know the parts of the flag is always `GCC` and a `{}`, we can use their hex values to find the phone number that has them. Similarly, we expect `_` to be in the flag so we also include its hex.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/7a0fa01f-fddb-477a-a428-78ad8a0aa284)

```
┌──(kali㉿kali)-[/mnt/hgfs/sharedfolder/gcc]
└─$ cat not_correct.txt | grep "4743437b"   
4743437b5233
                                                                                                                                                                            
┌──(kali㉿kali)-[/mnt/hgfs/sharedfolder/gcc]
└─$ cat not_correct.txt | grep "5f"      
6733785f347233
5f57316c64
                                                                                                                                                                            
┌──(kali㉿kali)-[/mnt/hgfs/sharedfolder/gcc]
└─$ cat not_correct.txt | grep "7d"
212121217d
```

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/82ae1b0e-26ef-4fc2-987d-2dae835fa132)
