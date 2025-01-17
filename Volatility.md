[Reference](https://blog.onfvp.com/post/volatility-cheatsheet/)

# Referencing Volatility in CLI  
- Volatility standalone executable for linux:  
- Volatility only launches with ‘volatility’ if installed via cli/apt-get.  
- Executable Standalone:  
  - May need to specify directory so the system specifies the file name vs command name as the commandlet is not installed.   
  - Use: ‘./volatility’ to specify the executable file vs command  
- Python standalone:  
  - May have to specify to launch with python, ‘python vol.py’ instead of ‘volatility’.  

  
## Commands:  
As we talked about above, you may need to adjust how you reference/call on Volatility.  
**Pull profile info (start here):** 

### Volatility -f memdump.mem imageinfo  

- Take memory image 'memdump.mem' and provide suggested profile info.  
- If volatility suggests a profile to use, be sure to:   
- -profile=*SuggestedProfile*

- Set Alias to this string (including the profile) to save time, see below  

### List processes:  

**PSlist**
- Volatility -f memdump.mem --profile=<Profile> pslist  
- Can pipe output to grep “<string>” or pipe again to wc -l for a word count  
- Take memory image, provide the profile, then use the ps list plugin to print a list of processes  
- Peints RUNNING processes  

**PStree**  
- Volatility –f memdump.mem --<Profile> pstree  
- Use the pstree plugin to print a process tree to terminal  

### List Commands run by user:  

**DLLlist**  
- CheckS commands used by process ID  
- Python vol.py ”<.mem path>” - -profile=<Profile> dlllist -p <Process ID>  
- Also, try the ’cmdscan’ tool for generic commands  
 
**Process Dump**   
- Volatility -f memdumb.mem - -profile=<Profile> procdump -p <processID> - -dump <DirPath>  
- View dump using: strings <dump> > strings.txt   
- This will help if you hit max results, then grep against file  
- You can also use memdump for processes (see below)  

 ### Other useful volatility plugins/tools:  

**PSscan**  
- Volatility –f memdump.mem --<Profile> psscan  
- Use the psscan plugin to print all available processes including hidden ones (typical to malware)  

**PSXview**  
- Volatility –f memdump.mem --<Profile> psxview  
- Use psxview plugin to print expected and hidden processes. Pslist+psscan.  

**Netscan**  
- Volatility –f memdump.mem --<Profile> netscan  
- Use netscan to identify any active or closed netowrk connections  
- Netscan plugin will show network communication per apps  
- Also check out ‘connscan’ for this  

**Timeline**  
- Volatility –f memdump.mem --<Profile> timeline  
- Use timeline to create a timeline of events from mem file  

**iEhistory**  
- Volatility –f memdump.mem --<Profile> iehistory  
- Use iehistory to pull internet browsing history  

**FileScan**  
- Volatility –f memdump.mem --<Profile> filescan  
- Use filescan to identify any files on the system from mem   
- Also check out ‘fileinfo’ for this  

**DumpFiles**  
- Volatility –f memdump.mem --<Profile> dumpfiles –n --dump-dir=./  
- Use dumpfiles to retrieve files from the mem.   
- Also check out ‘procdump’ for this  

**CMDscan**  
- Lists all observed commands  

1**HiveList**  
- Lists registry hives (location of)  
- Use ‘printkey’ to print key  
- Use ‘hivedump’ to print hive  

**Malfind** 
- Scans for injected code and potentially malicious processes  

**Clipboard**  
- Prints things cached in clipboard  

**Yarascan**  
- Scan mem with yara rule  

### Create Alias  
Consider setting an alias for the majority of this command to make your life easier (including the profile).  
- Alias vol=“<./volatility -f <memdump.mem> - -profile=<profile>”  

**Powershell**

- Set-alias -name <DesiredAlias> -value <commandstring>  
- Example:  
  - Alias vol="/home/ubuntu/Desktop/Tools/volatility3/vol.py -f /home/ubuntu/Desktop/Investigation/memdump.mem" - -profile=<profile>  
- This will allow you to run, ‘vol <command>’ for quick navigation  

**Troubleshooting**   

- No suitable address space mapping found  
  - Spaces in file path to the standalone or file names (including mem dump) may cause issues. Remove any spaces.  
**USE PROFILE!**  
- -profile=xxxxxxxxxxx  
- Other methods to get the suggested profile:  

- "imageint02" Plugin: As mentioned earier, 
  you Can use the ' 'image-info? plugin, Which 
  the Older plugin in 
  Volatility Running this plugin not only 
  "Wide information about the memory 
  image but also the 4>propriate 
  profiles for analysis.

**volatility3 -f <filepath> imageinfo2**

- KOBO Search: Volatility 
  for the Kernel Debugger alack 
  (KDBG) to determile the profile. You can 
  use the •kdbgscan• plugin to pertcrm tNs 
  get suggested profies.
**volatility -f <filepath> ksbgscan**

  - output Will display potential 
  based on the KDBG addresses. 
  - Volatility's Autodetect Feature: In some 
  cases, if the memory irnage is in the correct 
  format and the headers are intact, Volatility's 
  autodetect feature Suggest the profile 
  without needirg to specify a plugin 
  explicitly.
  
- verinfo  
- Hivelist  
- Older versions may not need a profile!!  
