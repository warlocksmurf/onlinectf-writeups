# Solution
## Task 1: Kitty
Question: Tetanus is a serious, potentially life-threatening infection that can be transmitted by an animal bite.

We are given a login page, so I analyzed the page source and stumbled across the Javascript file. It says that the credentials are `username` and `password`. Not very secure lol.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/af4ad084-579e-48c8-9f55-33f4d4cea505)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/be718818-c414-4ae2-8746-fe934088144f)

Then we are redirected to another page, the dashboard. Same thing I analyzed the page source and found a script field.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/ee5c4363-b538-4203-a853-5c91cdcbb6b8)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/18da3213-798d-4d08-85ad-2ec3b67f328b)

The script mentioned by using sending a command `cat flag.txt` in the input field, we can get the flag.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/782661e9-07fb-47b0-aebc-15cbac9d82c8)

## Task 2: Levi Ackerman
Question: Levi Ackerman is a robot!

We are given an image of Levi in the website, nothing else. So by reading the question, I did the common tactic in Web CTFs which is navigating to robots.txt.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/28351273-09c4-48a0-8fc3-505d41b94200)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/b66db858-ec5d-4857-970b-d24022204a9d)

We can see that it allows this URL, so we access it and get the flag.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/0dcdc787-202c-4a7e-b4bf-251d59b618ef)
