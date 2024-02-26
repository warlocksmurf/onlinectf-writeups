# Challenge 1: PLC I 🤖
Question: The MAPNA CERT team has identified an intrusion into the plant's PLCs, discovering a covert message transferred to the PLC. Can you uncover this secret message?

Flag: `MAPNA{y0U_sHOuLd_4lW4yS__CaR3__PaADd1n9!!}`

We are given a pcap file about PLC TCP packets. When analyzing the packets, we can notice some Ethernet packets have trailer paddings in them. This seems suspicious and after going through all the packets, the flag is obtained from their trailer. Another method is to just strings the pcap.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/212df183-f0b9-4da9-b918-f6daae977308)

```
1: MAPNA{y
2: 0U_sHOu
3: Ld_4lW4
4: yS__CaR
5: 3__PaAD
6: d1n9!!}
```

# Challenge 2: Tampered
Question: Our MAPNA flags repository was compromised, with attackers introducing one invalid flag. Can you identify the counterfeit flag?

Flag: `MAPNA{Tx,D51otN\\eUf7qQ7>ToSYQ\\;5P6jTIHH#6TL+uv}`

We are given a text file with multiple fake flags, I guess we have to find the outlier from it. One easy way is just to analyze them slowly and find the outlier (the memes were funny @Stoffe).

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/0747849c-dfa6-4ae2-aee2-0fa6fda98016)

![8d0vc6](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/37889b81-3169-4b80-bc4f-926dbf63f771)

# Challenge 3: XXG
Question: Welcome to the Forensics XXG challenge! Our investigator stumbled upon a mysterious file. Can you uncover the hidden message?

Flag: `MAPNA{F2FS_&_BFS_f1L3_5Ys73Ms_4rE_Nic3?!}`

For this challenge, I could not solve it before the CTF ended, but I knew it had something to do with hex byte editing so I attempted it after the CTF. We are given a weird file with XXG extension, analyzing the its data, we can find several information like .goutputstream, gimp-image-metadata and some unknown (???) data. 
GIMP strikes the hardest since most forensic challenges will require us to fix headers and obtain a GIMP-supported image,

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/5bcf8c94-d9d7-40e0-987a-ca5cefa2cf39)

While analyzing the file, we can find a hidden image file embedded within it. It is at the very bottom so we have to first extract the specific part and edit some header values.

```
dd if=MAPNA.XXG bs=1 skip=33556480 count=3953 of=MAPNA.xcf
```

Looking at this part, we can assume its:

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/93581e6c-5b56-41c0-ab43-dec99be914af)

```
GIMP 
Gimp ??? v014
gimp-image-grid
```

While reading on Gimp v014, we stumbled upon this website which mentions that its an XCF file. Since we know its an XCF file now, we can study its [headers](https://developer.gimp.org/core/standards/xcf/#header)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/bee412a3-0ce1-43e6-93e4-bf24d6ecda3b)

We can start fixing the XCF header and after that, open it on GIMP to get the flag.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/c3af5672-f299-45f1-9f01-0842f88b77f2)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/3e0e03c6-1115-4b49-9617-2fdd3e5a34e1)
