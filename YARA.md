In depth how to use Yara rules
Metadata: (does not affect the rule itself)
Author – Name, Email, X Handle
Date – Creation date
Version – Tracking amendments
Reference – Link to article or sample
Description – brief overview of the rules purpose and malware aiming to detect
Hash – list of sample hashes that were used to create rule

**Strings**: Defines search criteria
The string from the malware needs to be declared a variable
A$="string from malware sample"

**Modifiers**:
A$="malwarestring" **fullword** match exact word
A$="malwarestring" **wide** match unicode strings seperated bu null bytes ex: '.m.a.l.w.a.r.e.c.o.m.'
A$="malwarestring" **wide ascii** rule matches on acsii and unicode characters
A$="malwarestring" **nocase** rule matches string regardless of case

![image](https://github.com/user-attachments/assets/8d3e749f-ddad-4d0c-9ab1-7598c2ecac17)
Using the hexadecimal from photo.png
A$={5C 70 68 6F 74 6F 2E 70 6E 67}
A$={5C 70 68 6F ?? ?F 2E 70 6E 67}  use ? For a wildcard for slight variation of a hex pattern
A$={5C [2-10] 68 6F 74 6F 2E 70 6E 67}  may be 2-10 random bytes before a matching pattern begins
$a={5C (01 02 | 03 04) 6F 2E 70 6E 67}  hex in location could be '01 02' or '03 04'

Some great strings for Yara rules:
**Mutexes** – Unique to malware families, used to check if a device has already been compromised by checking for the presence of a mutex
**Rare and unusual user agents** – communication with C2 infrastructure
**Registry Keys** – created by malware as a persistence mechanism
**PDB Paths** – Program Database, PDB contains debugging information about a file. Unlikely to have a PDB for malware
**Encrypted config strings** – Malware often encrypts config files containing useful IOCs, IPs, and domains. Reverse engineer to find the encrypted data can be used in Yara rules

**Conditions**: Define criteria to trigger a successful match
Can use multiple conditions
Uint16(0) == 0x5A4D checks the header for Window's executable. Always at the start of the hex values for Windows executables. Reversed due to endianness https://en.wikipedia.org/wiki/Endianness
Uint32(0) == 0464c457f or uint32(0)==0xfeedfacf or uint32(0)==0xcffaedfe or uint32(0)==feedface or uint32(0)==0xceffaedfe – used to ID linux binaries by checking file header
(#a==6) - string count equal to 6
(#a>6) string could greater than 6
(filesize>512)
(filesize<5000000)
(filesize<5MB)

Once strings have been declared within a rule you can then customize how many matches need to be triggered as a condition for the rule to return what it deems a successful condition:
2 of ($a,$b,$c)
3 of them
4 of ($a*)
All of them
Any of them
$a and not $b
Try to use 2-3 groups of conditions to avoid generating false positives and also create a reliable rule.

**Imports**:
You can use PEStudio to find the imports and exports and create rules from them:
![image](https://github.com/user-attachments/assets/0a8c1db4-b69e-4eb1-ad8b-6f2fb388877f)
Pe.exports("Botanist", "Chechako", Originator", "Repressions")

Interesting dll that is used for http connections winhttp.dll:
![image](https://github.com/user-attachments/assets/bfa88a63-4a90-4a3b-a025-697d1270e637)

You can use the blacklisted imports to create rules:
![image](https://github.com/user-attachments/assets/b8c56e73-ffb0-4a80-86c4-0765854eaea3)
Pe.imports("winhttp.dll", "WinHttpConnect")
Pe.machine == pe.MACHINE_AMD64 used for checking machine type

**Imphash** is the hash of the malware's import address table IAT which can be see in PEStudio
The same IAT will often be used across a malware family so using it in yara should detect similar samples
Pe.imphash() ==  "0E18F33408BE6E4CB217F0266066C51C”

**Compiler timestamps**:
// means adding a comment
Pe.timestamp == 1616850469 // Tue Dec 08 17:58:56 2020

**CompanyName field**:
Pe.version_info["CompanyName"] contains AmAZon.cOm
Pe.language(0x0804) // China – Languages identified can be used by specifying the Microsoft language code 
[link](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-lcid/70feba9f-294e-491e-b6eb-56532684c37f)
![image](https://github.com/user-attachments/assets/0dbd0fe7-b69e-46ac-af1c-ec854a240ec7)

You can do sections within PEStudio:
Pe.sections[2].name == "BSS"
![image](https://github.com/user-attachments/assets/2946cf92-162a-4b9d-921f-52b5e3b0ee64)
Section 0 is CODE, Section 1 is DATA, section 2 is BSS 
