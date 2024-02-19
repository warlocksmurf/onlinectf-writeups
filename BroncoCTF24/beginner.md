# Solution
## Task 1: Keyboard Elitist
Question: My buddy is bragging about how cool his Framework laptop is and how much faster he can type than me. When I tried to type a message, it came out as garbage!
```
A;;apfkgij gj;ukd ar ut ghur war a Qwfpgj efjbyaps yk a Cyifmae uk;lg rchfmf maefr ghur iyye iuef dapbadf. Mj tpufks ur sftukugfij a efjbyaps rkyb, ylg hfpf wugh hur mysliap tpamfwype ia;gy;. Rudh, fughfp waj... hfpf ur ghf tiadO bpykcy{qwfpgj_vr_c0ifm@e}
```

Flag: `bronco{qwerty_vs_c0lem@k}`

At first I was stuck at this for quite awhile, but after reading the description carefully, it could suggest that this was a different keyboard layout. Using dcode, I noticed that the encoded text was actually [Colemak](https://colemak.com/) which I have never heard of before.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/852ce8c4-14e5-46a1-a2d1-d5a7c6d8f3b6)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/01b2b2c5-356d-48c7-bc03-e94c00f50c91)

## Task 2: Shrekanana Banana
Question: I was given this image of Shrek in a Banana, but I can't help but feel like I am missing something...

Flag: `bronco{shr3konogr@phy}`

We were given an image file of Shrek banana, just use some generic steganography tool like [Aperi'Solve](https://www.aperisolve.com/) to analyze the file.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/801d0155-8a54-416a-b8e2-f988eba79c68)

## Task 3: Stego-Snore-Us
Question: I'm not the only one tired after pulling an all-nighter for Hack for Humanity...

Flag: `bronco{no_more_all_nighters}`

We were given an image file of a sleeping dinosaur, just use some generic steganography tool like [Aperi'Solve](https://www.aperisolve.com/) to analyze the file. However this time the flag was encoded, so I took the encoded flag and analyzed it slowly.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/b55a0b13-d859-4cdc-9c56-6d509abe960f)

Since we know pesxas should be bronco, it suggests a mono-alphabetic cipher. I used a good online substitution tool called [quipquip](https://quipqiup.com/) and got the flag.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/0a2583be-ea62-494b-955a-385631dc6e17)
