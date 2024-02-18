# Solution
## Task 1: Where's skat?
While traveling over the holidays, I was doing some casual wardriving (as I often do). Can you use my capture to find where I went? Note: the flag is irisctf{the_location}, where the_location is the full name of my destination location, not the street address. For example, irisctf{Washington_Monument}. Note that the flag is not case sensitive.

Flag: `irisctf{los_angeles_union_station}`

We are given a pcap file, and since we are asked where they went, (the destination specifically) we can go to the end of the PCAP and look around. 
We can notice them communicating with nearby routers and it seems to lead to the same place everytime.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/26b20a78-c1fd-49fe-bb3d-04d5c0cc1cb8)

From what I understand, it seems that skat is being in some station as it shows multiple Metro SSIDs and by checking LAUS with Morlin together (I thought Morlin was a hotel), we can get the location.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/909b75c1-7607-4289-8684-3204db5c5fd4)

## Task 2: skat's Network History
Question: "I love cats." Note: this challenge is a continuation to Forensics/skat's SD Card. You are dealing with the same scenario. skats-sd-card.tar.gz is the same file from that challenge (SHA-1: 4cd743d125b5d27c1b284f89e299422af1c37ffc).

Flag: `irisctf{i_sp3nd_m0st_of_my_t1me_0n_th3_1nt3rnet_w4tch1ng_c4t_v1d30s}`

For this challenge, I could not solve it before the CTF ended, but @seal taught me how so thanks! We are given a pcap file again and some encrypted 802.11 packets. Poking around the skat's directory in the SD card from the Forensics challenge, the Bookshelf folder has a PDF manual for setting up a Raspberry Pi. So checking on the `/etc/NetworkManager/system-connection/skatnet.nmconnection` file, we can see it contains a WiFi password.

```
[connection]
id=skatnet
uuid=470a7376-d569-444c-a135-39f5e57ea095
type=wifi
interface-name=wlan0

[wifi]
mode=infrastructure
ssid=skatnet

[wifi-security]
auth-alg=open
key-mgmt=wpa-psk
psk=agdifbe7dv1iruf7ei2v5op

[ipv4]
method=auto

[ipv6]
addr-gen-mode=default
method=auto

[proxy]
```

With the PSK, we can decrypt the packets using Wireshark by navigating to `Preferences > Protocols > IEEE 802.11 > Edit` to add the PSK as the wpa-pwd.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/d104bc85-6755-43d1-bf31-748e8f9387b9)

The other file we are given is some sslkeys, so we can utilize them by navigating to Protocols > TLS and setting the (Pre)-Master-Secret log file to the sslkeyfile allows us to read the HTTPS traffic.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/70706240-3ede-42a9-b61b-8de9cb532214)

Looking through the traffic, we can find HTTP/2 (first time hearing about this) and analyzing these packets, we can find a pastebin entry. Going through the HTTP2 streams, we find the flag.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/b099cd64-d735-4dc5-9424-05cb9756af1e)
