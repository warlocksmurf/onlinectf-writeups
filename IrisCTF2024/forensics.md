# Solution
## Task 1: Not Just Media
Question: I downloaded a video from the internet, but I think I got the wrong subtitles. Note: The flag is all lowercase.

We are given a weird .mkv video file and reading the scenario, we have to probably analyze the subtitles. So doing my research online, we can use a tool called `mkvinfo`. Using the tool, we can find there are multiple tracks and subtitles embedded within this video.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/73facf85-395c-4be4-bafa-d7344fc85824)

Analyzing the metadata, we notice 1 of the font files called FakeFont.ttf (kinda suspicious). So we extract this font file and also the subtitle of the video.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/6d7bad50-4c28-41d4-b3a6-a28e041b226a)

In the subtitles file, we can find the subtitles being several chinese letters. Since we already have the font file, we just have to combine them together using an online tool called `fontdrop.io`.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/97222741-0790-40ca-ab72-29fdc153e6fc)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/edc43207-90cc-4854-9dd1-cb907a6e2b3f)
