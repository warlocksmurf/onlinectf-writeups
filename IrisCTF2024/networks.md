# Solution
## Task 1: Where's skat?
Question: While traveling over the holidays, I was doing some casual wardriving (as I often do). Can you use my capture to find where I went? 
Note: the flag is irisctf{the_location}, where the_location is the full name of my destination location, not the street address.

We are given a pcap file, and since we are asked where they went, (the destination specifically) we can go to the end of the PCAP and look around. 
We can notice them communicating with nearby routers and it seems to lead to the same place everytime.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/26b20a78-c1fd-49fe-bb3d-04d5c0cc1cb8)

From what I understand, it seems that skat is being in some station as it shows multiple Metro SSIDs and by checking LAUS with Morlin together (I thought Morlin was a hotel), we can get the location.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/909b75c1-7607-4289-8684-3204db5c5fd4)

