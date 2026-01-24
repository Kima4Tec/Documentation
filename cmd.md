# Command prompts and others good to know

### shell:recent
shows all recent files opened

### shell:downloads
shows all recent downloads

### shell:personal
shows folder documents

### appwiz.cpl
Add and remove programs

### msinfo32
shows pc-info

### get-computerinfo
shows pc-info using powershell

### Get-CimInstance Win32_VideoController
shows graphic card info

### windows + x
shows power user menu

### winget upgrade --all
upgrades all apps on system

### SFC /scannow
scans and cleans system

### DISM /Online /Cleanup-Image /RestoreHealth
Deeper scan and fix

### powercfg /batteryreport
creating report about battery for laptops

### ipconfig /displaydns
shows dns cache and debugs weird website issues

### netstat -ano
For network engineers: what is using what port

### netstat -ano | findstr :4200
Who is using port 4200

### taskkill /PID 12345 /F
If netstat -ano | findstr :4200 shows:
```
TCP    127.0.0.1:4200    LISTENING    12345
```
you can kill that instance

### tasklist /svc
shows a list of tasks on the system with pid

### shutdown /s /t 0
Shutdown with time

### shutdown /i
shutdown with graphic

### getmac
shows mac adress of local and remote machines

### wmic product get name
gives a list of all app on system

### gpupdate /force
updates group policy

### net use \\computer\c$

### whoami
my user profile

### whoami /groups
shows what groups I am in

### choco install <packagename>
shows chocolately for software install

### taskkill /f /im, explorer.exe & start explorer.exe 
fix frozen taskbar

## Restore Password on pc
1. click shift on lock screen
2. select restart
3. still holding shift - when blue screen, click on troubleshoot, and then advanced options
4. On next screen type:
```
c:
cd windows
cd system32
ren utilman.exe utilman.exe
ren cmd.exe utilman.exe
exit
```
5. click on 'continue'
6. go back to lock screen and click on little person opening prompt menu
7. type and open user account where you can select reset password
```
control userpasswords2
```
8. change password on lockscreen and regain access





