# Scenario: DFIR (Memory Analysis)
My boss, Muhammad, sent me this dump file of a memory. He told me that this OS has a malware virus that runs automatically. I need to find some more information about this OS, and the hacker also created some files in this OS. He gave me a task to solve this within 24 hours. I am afraid. Will you please help me? My boss sent some questions; please solve them on my behalf. There are total 7 challenges in this series. Best of luck.

## Task 1: OS
Question: What is the OS version?

Use Windbg with this command `!analyze -v`.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/9443cf5f-d9a6-4a67-b592-690b6c68faf5)

## Task 2: Password
Question: What is the login password of the OS? 

Use volatility2 hashdump plugin to get password hashes, then use crackstation.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/f33b1bf8-a047-4d05-87bd-624dff7b1679)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/395ccfb2-e3a7-4bb4-b3a3-acc706854da0)

## Task 3: IP Addr
Question: What is the IP address of this system?

Use Windbg with this command `du poi(poi(srvnet!SrvAdminIpAddressList))` and keep `spamming du`.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/744eaa9b-1602-4881-ac81-914355cb82cd)

## Task 4: Note
Question: My boss has written something in the text file. Could you please help me find it? 

Use volatility2 and dump the suspicious text file and you can find an encoded flag.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/de8e9e81-bf8f-4d43-8ede-ef191801f43a)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/57d5b61e-fbf9-4b1c-b8f4-d804ac19b2ed)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/b3e7dbaa-a57d-42e2-8a71-6e5560e3f9de)

## Task 5: Execution
Question: My leader, Noman Prodhan, executed something in the cmd of this infected machine. Could you please figure out what he actually executed? 

I just strings grep the memory dump and found the flag lol.
Intended way: Use volatility2 consoles plugin and find the executable and command.

![KNIGHT](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/cede368f-8aa7-4fd5-8595-4ad8ea1d695e)

## Task 6: Path of the Executable
Question: What is the path folder of the executable file which execute privious flag? 

Similarly, use volatility2 consoles plugin and find the executable folder location.

![KNIGHT](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/cede368f-8aa7-4fd5-8595-4ad8ea1d695e)

## Task 7: Malicious
Question: What is the malicious software name? 

Thanks @Hyper Nova, use volatility2 autoruns plugin, a suspicious software can be seen.

![KNIGHT2](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/6da1fc9a-a7a1-4765-87e4-9a2475ee3750)
