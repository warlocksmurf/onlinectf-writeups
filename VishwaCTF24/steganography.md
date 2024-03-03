## Task 1: We Are Valorant
Question: One day, while playing Valorant, I received a mail from Riot Games that read,
"In a world full of light, sometimes the shadows help you win." "Your Signature move also helps you a lot ; develop one and ace it now."
It also had an image and a video/gif attached to it. I am not able to understand what they want to say. Help me find what the message wants to express.

Flag: `VishwaCTF{you_are_invited_to_the_biggest_valorant_event}`

We are given a corrupted jpg file and a mp4 video of Astra (which seems to be a gif). Fixing the jpg file, it shows the agents in Valorant but there were no important information in it. Thinking this was a steganography challenge, I went ahead to analyze the jpg file via [Aperi'Solve](https://www.aperisolve.com/) and found out the common passwords used by other people. One of them was `Tenz` which I had no clue how they gotten it but it was the password to extract a hidden txt file.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/7910cba1-3669-4e00-b63e-11dec23cc11b)

The text file contains the flag.

```
Hello!!
hope you are enjoying the CTF
here's your flag

VishwaCTF{you_are_invited_to_the_biggest_valorant_event}
```

## Task 2: Mysterious Old Case
Question: You as a FBI Agent, are working on a old case involving a ransom of $200,000. After some digging you recovered an audio recording.

Flag: `VishwaCTF{1_W!LL_3E_B@CK}`

We are given a mp3 file that seems to be just music. However, after I analyzed it on Audacity, it seems that there were hidden messages in them.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/511d1a18-1fdd-47c4-afb4-f78e1c09c8e3)

Listening on the part, it seems that the voice message is reversed, so I reversed the whole mp3 file and used this [site](https://www.veed.io/) to get the transcript of the reversed mp3 file.

The transcript:
```
Cooper.
It is 24 November 1971.
Now I have left from Seattle and headed towards Reno.
I have got all my demands fulfilled.
I have done some changes in the flight log and uploaded it to the remote server.
The file is encrypted.
The hint for description is the airline I was flying in.
Most importantly, the secret key is split and hidden in every element of the Fibonacci series, starting from two.
```

Here, I was stuck since I have no clue where the encrypted file was. However, I suddenly found a suspicious Google Drive link in the metadata of the mp3 file.

```
└─$ exiftool final.mp3
ExifTool Version Number         : 12.76
File Name                       : final.mp3
Directory                       : .
File Size                       : 2.6 MB
File Modification Date/Time     : 2024:03:02 23:19:36-05:00
File Access Date/Time           : 2024:03:03 04:14:51-05:00
File Inode Change Date/Time     : 2024:03:02 23:19:36-05:00
File Permissions                : -rwxrwxrwx
File Type                       : MP3
File Type Extension             : mp3
MIME Type                       : audio/mpeg
MPEG Audio Version              : 1
Audio Layer                     : 3
Audio Bitrate                   : 128 kbps
Sample Rate                     : 44100
Channel Mode                    : Stereo
MS Stereo                       : Off
Intensity Stereo                : Off
Copyright Flag                  : False
Original Media                  : False
Emphasis                        : None
ID3 Size                        : 320209
Title                           : Unknown
Artist                          : Anonymous
Track                           : 727/305
Album                           : Cooper
Recording Time                  : 1971
Genre                           : the zip file is 100 MB not 7 GB
Original Release Time           : 0001
Band                            : DB Cooper
Comment                         : password for the zip is all lowecase with no spaces
User Defined URL                : https://drive.google.com/file/d/1bkuZRLKOGWB7tLNBseWL34BoyI379QbF/view?usp=drive_lin
User Defined Text               : (purl) https://drive.google.com/file/d/1bkuZRLKOGWB7tLNBseWL34BoyI379QbF/view?usp=drive_lin
Picture MIME Type               : image/jpeg
Picture Type                    : Front Cover
Picture Description             : Front Cover
Picture                         : (Binary data 158421 bytes, use -b option to extract)
Date/Time Original              : 1971
Duration                        : 0:02:22 (approx)
```

The URL link gives us a zip file of the flight logs. As the question mentioned, the password is the airline Cooper was flying in. Thankfully, I am a huge fan of the story of DB Cooper so I knew the airline name was `Northwest Orient Airlines`. However, the comment metadata mentioned the password being all lowercased with no spaces, so the password is `northwestorientairlines`.

Extracting the files in the zip, many flight logs can be obtained. Since I knew his story already, the flight he was in was `Flight #305`. Reading the log files, it seems that the flag was broken up into pieces with bogus text between them. The question mentioned about Fibonacci sequence which seems to be the case where parts of the flag was following the sequence starting from 2. `(2, 3, 5, 8, 13, 21, 34, 55, 89,144,233,377,610,987, 1597, 2584, 4181, etc)`

```
1971-11-24 06:22:08.531691 - ATT - Boeing 727
V
i
1971-11-24 07:31:08.531691 - HWR - Boeing 727
s
1971-11-24 06:22:08.531691 - ATT - Boeing 727
1971-11-24 07:31:08.531691 - HWR - Boeing 727
h
1971-11-24 06:22:08.531691 - ATT - Boeing 727
1971-11-24 06:22:08.531691 - ATT - Boeing 727
1971-11-24 06:22:08.531691 - ATT - Boeing 727
1971-11-24 07:31:08.531691 - HWR - Boeing 727
w
1971-11-24 07:31:08.531691 - HWR - Boeing 727
1971-11-24 06:22:08.531691 - ATT - Boeing 727
1971-11-24 06:22:08.531691 - ATT - Boeing 727
1971-11-24 07:31:08.531691 - HWR - Boeing 727
1971-11-24 07:31:08.531691 - HWR - Boeing 727
1971-11-24 07:31:08.531691 - HWR - Boeing 727
1971-11-24 06:22:08.531691 - ATT - Boeing 727
a
...
```

However, one good thing the authors missed out on was the bogus text were repeated, so I just created a script to remove them and the flag can be obtained.

```
def remove_entries(log_filename, entries_to_remove):
    with open(log_filename, 'r') as file:
        lines = file.readlines()

    with open(log_filename, 'w') as file:
        for line in lines:
            if not any(entry in line for entry in entries_to_remove):
                file.write(line)

log_filename = "./flight_logs/Flight-305.log"

entries_to_remove = [
    "1971-11-24 06:22:08.531691 - ATT - Boeing 727",
    "1971-11-24 07:31:08.531691 - HWR - Boeing 727"
]

remove_entries(log_filename, entries_to_remove)
print("Entries removed successfully.")
```

```
V
i
s
h
w
a
C
T
F
{
1
_
W
!
L
L
_
3
E
_
B
@
C
K
}
```

## Task 3: Secret Code
Question: Akshay has a letter for you and need your help.

Flag: `VishwaCTF{th15_15_4_5up3r_53cr3t_c0d3_u53_1t_w153ly_4nd_d0nt_5h4re_1t_w1th_4ny0ne}`

The question gave us a black jpg file and a txt file containing a message from someone.

```
To,
VishwaCTF'24 Participant

I am Akshay, an ex employee at a Tech firm. Over all the years, I have been trading Cypto currencies and made a lot of money doing that. Now I want to withdraw my money, but I'll be charged a huge tax for the transaction in my country.

I got to know that you are a nice person and also your country doesn't charge any tax so I need your help. 

I want you to withdraw the money and hand over to me. But I feel some hackers are spying on my internet activity, so I am sharing this file with you. Get the password and withdraw it before the hackers have the access to my account.

Your friend,
Akshay
```

The first thing I did was binwalk the jpg file to extract hidden data embedded within it.

```
└─$ binwalk -e confidential.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01
116247        0x1C617         Zip archive data, at least v2.0 to extract, compressed size: 72486, uncompressed size: 72530, name: 5ecr3t_c0de.zip
188778        0x2E16A         Zip archive data, at least v2.0 to extract, compressed size: 170, uncompressed size: 263, name: helper.txt
189177        0x2E2F9         End of Zip archive, footer length: 22
```

The zip file contains a password-protected zip file and a txt file containing a hint.

```
Hey buddy, I'm really sorry if this takes long for you to get the password. But it's a matter of $10,000,000 so I can't risk it out.

"I really can't remember the password for zip. All I can remember is it was a 6 digit number. Hope you can figure it out easily"
```

So it was obvious I had to crack the zip password. I used `fcrackzip` but it could not crack it, so I used `John the Ripper` instead. I also created a script to generated a wordlist of 6 digit numbers for a quicker crack.

```
$ zip2john 5ecr3t_c0de.zip > secret.hash
$ john --wordlist=sixdigits.txt secret.hash
$ john --show secret.hash

5ecr3t_c0de.zip/5ecr3t_c0de.txt:945621:5ecr3t_c0de.txt:5ecr3t_c0de.zip:5ecr3t_c0de.zip
5ecr3t_c0de.zip/info.txt:945621:info.txt:5ecr3t_c0de.zip:5ecr3t_c0de.zip

2 password hashes cracked, 0 left
```

After unlocking the zip file, two txt files can be obtained. One of them was random numbers while the other had another hint.

```
What are these random numbers? Is it related to the given image? Maybe you should find it out by yourself
```

Analyzing the random numbers, they seem to either be (x,y) coordinates or (x,y) pixels.

```
(443, 1096)
(444, 1096)
(445, 1096)
(3220, 1096)
(3221, 1096)
(38, 1097)
(39, 1097)
(43, 1097)
(80, 1097)
(81, 1097)
(83, 1097)
(93, 1097)
(95, 1097)
...
```

Since the hint mentioned something about the given image (confidential.jpg), I used ChatGPT to create a script that will color a white pixel on a blank jpg file which could uncover the flag.

```
from PIL import Image

def read_coordinates(file_path):
    with open(file_path, 'r') as f:
        lines = f.readlines()
    coordinates = []
    for line in lines:
        x, y = line.replace('(', '').replace(')', '').split(',')
        coordinates.append((int(x), int(y)))
    return coordinates

def draw_white_pixels(image_path, coordinates, output_path):
    img = Image.open(image_path)
    for coord in coordinates:
        img.putpixel(coord, (255, 255, 255))  # RGB value for white
    img.save(output_path)

image_path = '/confidential.jpg'
file_path = '/5ecr3t_c0de.txt'
output_path = 'output_image.png'

coordinates = read_coordinates(file_path)
draw_white_pixels(image_path, coordinates, output_path)

print(f"Image with white pixels at the given coordinates has been saved as {output_path}.")
```

After running the script, the flag is shown in the output jpg file.

![output_image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/b9e33a42-f20f-4553-b7fc-137099bf6b1a)
