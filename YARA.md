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

**Modifiers:
A$="malwarestring" **fullword** match exact word
A$="malwarestring" **wide** match unicode strings seperated bu null bytes ex: '.m.a.l.w.a.r.e.c.o.m.'
A$="malwarestring" **wide ascii** rule matches on acsii and unicode characters
A$="malwarestring" **nocase** rule matches string regardless of case

![image](https://github.com/user-attachments/assets/8d3e749f-ddad-4d0c-9ab1-7598c2ecac17)
