# Scenario
"We used to be peaceful and had enough tech to keep us all happy. What do you think about that? 

These data disks alluded to some "societal golden age." No fighting, no backstabbing, and no factions fighting for some lousy title.

Good, great for them-

Because all we get to look forward to is "The Fray."

alarms blaring

Oh, look-... it's showtime." (Quote: Luxx, faction leader of the Phreaks)

💥 Welcome to "The Fray." A societal gauntlet made of the most cunning, dedicated, and bloodthirsty factions. We are all bound by the same rule–be one of the last factions standing. All brought to your overlords and sponsors at KORP™.

Our city's lights bring people from far and wide. It's one of the last remaining mega structures left after the Great Division took place. But, as far as we are concerned, KORP™ is all there ever was and will be. 

They hold The Fray every four years to find the "best and the brightest around." Those who make it through their technological concoction of challenges become the "Legionaries," funded factions who get to sit on easy-street for the time between the next fight.

## Task 1: Maze
Question: In a world divided by factions, "AM," a young hacker from the Phreaks, found himself falling in love with "echo," a talented security researcher from the Revivalists. Despite the different backgrounds, you share a common goal: dismantling The Fray. You still remember the first interaction where you both independently hacked into The Fray's systems and stumbled upon the same vulnerability in a printer. Leaving behind your hacker handles, "AM" and "echo," you connected through IRC channels and began plotting your rebellion together. Now, it's finally time to analyze the printer's filesystem. What can you find?

Flag: `HTB{1n7323571n9_57uff_1n51d3_4_p21n732}`

We are given a weird folder. Analyzing the folder, a pdf file can be found in one of the folders and the flag is within it.

```
PS C:\Users\ooiro\Downloads\hardware_maze> tree /F
Folder PATH listing
Volume serial number is 8A11-77EB
C:.
└───fs
    ├───PJL
    ├───PostScript
    ├───saveDevice
    │   └───SavedJobs
    │       ├───InProgress
    │       │       Factory.pdf
    │       │
    │       └───KeepJob
    └───webServer
        ├───default
        │       csconfig
        │
        ├───home
        │       device.html
        │       hostmanifest
        │
        ├───lib
        │       keys
        │       security
        │
        ├───objects
        └───permanent
```

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/b86a5422-e574-4056-aa38-bf55b47ac1ee)

## Task 2: BunnyPass
Question: As you discovered in the PDF, the production factory of the game is revealed. This factory manufactures all the hardware devices and custom silicon chips (of common components) that The Fray uses to create sensors, drones, and various other items for the games. Upon arriving at the factory, you scan the networks and come across a RabbitMQ instance. It appears that default credentials will work.

Flag: `HTB{th3_hunt3d_b3c0m3s_th3_hunt3r}`

We are given a `RabbitMQ` instance to investigate. On the login page, we use the default credentials of `admin` username and `admin` password. Inside the website portal, the flag is probably hidden somewhere. 

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/6241c200-afc4-43f7-8e4f-597d3a677d1c)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/b1ad4d54-651d-4558-815b-ec445f68e445)

After 2 hours, I came across the `Queues` page where it seems that there are several messages in certain queues. Analyzing the `factory_idle` queue, I noticed the `Get Message` function.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/b2a840d2-2aa6-44d5-af54-276db4dfbf0d)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/81dab5fa-593b-42ab-b4ac-614c73468846)

Since the queue has 6 messages, we can use the function to output the messages and the flag can be obtained.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/cefb30b8-1528-417d-ae09-1058e73c607a)
