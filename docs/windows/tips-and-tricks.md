# Windows Command Line Tips and Tricks

A comprehensive guide to command line productivity on Windows, covering CMD, PowerShell, and general tips for day-to-day work.

## Table of Contents
- [Command Prompt (CMD) Essentials](#command-prompt-cmd-essentials)
- [PowerShell Essentials](#powershell-essentials)
- [File and Directory Operations](#file-and-directory-operations)
- [System Information and Management](#system-information-and-management)
- [Networking Commands](#networking-commands)
- [Process Management](#process-management)
- [Productivity Tips](#productivity-tips)
- [Environment Variables](#environment-variables)
- [Shortcuts and Aliases](#shortcuts-and-aliases)
- [Advanced Tricks](#advanced-tricks)
- [Windows Subsystem for Linux (WSL)](#windows-subsystem-for-linux-wsl)

---

## Command Prompt (CMD) Essentials

### Basic Navigation
```cmd
:: Change directory
cd C:\Users\YourName\Documents

:: Change drive
D:

:: Go up one directory
cd ..

:: Go to home directory
cd %USERPROFILE%

:: Show current directory
cd

:: List files and folders
dir

:: List with details
dir /w    :: Wide format
dir /p    :: Pause after each screen
dir /s    :: Include subdirectories
dir /a    :: Show hidden files
dir /o:n  :: Order by name
dir /o:d  :: Order by date
```

### Quick Tips
```cmd
:: Clear screen
cls

:: View command history
doskey /history

:: Copy output to clipboard
ipconfig | clip

:: Run command and pause
command && pause

:: Run as administrator (from Run dialog: Win+R)
:: Type: cmd
:: Press: Ctrl+Shift+Enter
```

---

## PowerShell Essentials

### Starting PowerShell
```powershell
# Open PowerShell from CMD
powershell

# Open PowerShell as Admin (from Run: Win+R)
# Type: powershell
# Press: Ctrl+Shift+Enter

# Open PowerShell at current location (in File Explorer)
# File -> Open Windows PowerShell
```

### Basic PowerShell Commands
```powershell
# List files and folders (PowerShell style)
Get-ChildItem
ls        # Alias
dir       # Alias

# List with hidden files
Get-ChildItem -Force

# Navigate
Set-Location C:\Users
cd C:\Users    # Alias

# Get current location
Get-Location
pwd           # Alias

# Clear screen
Clear-Host
cls           # Alias

# Get command history
Get-History
history       # Alias
```

### PowerShell-Specific Features
```powershell
# Get help for any command
Get-Help Get-ChildItem
Get-Help Get-ChildItem -Examples
Get-Help Get-ChildItem -Full

# Find commands
Get-Command *process*
Get-Command -Noun Process

# See command aliases
Get-Alias
Get-Alias -Name ls
Get-Alias -Definition Get-ChildItem

# Measure command execution time
Measure-Command { Get-ChildItem -Recurse }

# Copy output to clipboard
Get-Process | clip
```

---

## File and Directory Operations

### Creating and Deleting
```cmd
:: CMD: Create directory
mkdir newfolder
md newfolder

:: Create nested directories
mkdir parent\child\grandchild

:: Create file
echo. > newfile.txt
type nul > newfile.txt

:: Delete file
del filename.txt
erase filename.txt

:: Delete directory
rmdir foldername
rd foldername

:: Delete directory and contents
rmdir /s foldername
rd /s /q foldername   :: Quiet mode (no confirmation)
```

```powershell
# PowerShell: Create directory
New-Item -ItemType Directory -Name newfolder
mkdir newfolder    # Alias

# Create file
New-Item -ItemType File -Name newfile.txt
echo $null > newfile.txt

# Delete file
Remove-Item filename.txt
del filename.txt    # Alias

# Delete folder recursively
Remove-Item foldername -Recurse -Force
```

### Copying and Moving
```cmd
:: CMD: Copy file
copy source.txt destination.txt

:: Copy multiple files
copy *.txt D:\backup\

:: Copy directory and contents
xcopy sourcedir destdir /e /i
robocopy sourcedir destdir /e    :: More powerful

:: Move file
move source.txt destination.txt

:: Rename file
ren oldname.txt newname.txt
rename oldname.txt newname.txt
```

```powershell
# PowerShell: Copy file
Copy-Item source.txt destination.txt
cp source.txt destination.txt    # Alias

# Copy directory recursively
Copy-Item sourcedir destdir -Recurse

# Move file
Move-Item source.txt destination.txt
mv source.txt destination.txt    # Alias

# Rename file
Rename-Item oldname.txt newname.txt
ren oldname.txt newname.txt      # Alias
```

### Searching Files
```cmd
:: CMD: Find files
dir /s filename.txt
dir /s *.pdf

:: Search file contents
find "search term" filename.txt
findstr "pattern" *.txt
findstr /s /i "pattern" *.*    :: Recursive, case-insensitive

:: Where is a command located
where python
where notepad
```

```powershell
# PowerShell: Find files
Get-ChildItem -Recurse -Filter "*.txt"
Get-ChildItem -Recurse | Where-Object {$_.Name -like "*pattern*"}

# Search file contents
Select-String -Path "*.txt" -Pattern "search term"
Select-String -Path "*.txt" -Pattern "search term" -Recurse

# Where is a command
Get-Command python
Get-Command notepad
```

---

## System Information and Management

### System Information
```cmd
:: CMD: System info
systeminfo
systeminfo | findstr /C:"OS Name" /C:"OS Version"

:: Computer name and user
hostname
whoami
echo %COMPUTERNAME%
echo %USERNAME%

:: Windows version
ver
winver    :: GUI version

:: Check if 32 or 64 bit
wmic os get osarchitecture
echo %PROCESSOR_ARCHITECTURE%

:: View environment variables
set
set PATH
```

```powershell
# PowerShell: System info
Get-ComputerInfo
Get-ComputerInfo | Select-Object WindowsVersion, OsArchitecture

# OS version
$PSVersionTable
[System.Environment]::OSVersion

# Environment info
Get-ChildItem Env:
$env:PATH
$env:USERNAME
```

### Disk Management
```cmd
:: CMD: Check disk space
wmic logicaldisk get name,size,freespace
dir C:\

:: Check disk for errors (requires admin)
chkdsk C:
chkdsk C: /f    :: Fix errors

:: Disk cleanup
cleanmgr
```

```powershell
# PowerShell: Disk space
Get-PSDrive
Get-PSDrive -PSProvider FileSystem

# Detailed disk info
Get-Volume
Get-Disk

# Folder size
(Get-ChildItem -Recurse | Measure-Object -Property Length -Sum).Sum / 1GB
```

---

## Networking Commands

### Network Information
```cmd
:: CMD: IP configuration
ipconfig
ipconfig /all

:: Release and renew IP
ipconfig /release
ipconfig /renew

:: Flush DNS cache
ipconfig /flushdns

:: Show network connections
netstat
netstat -ano     :: Show all connections with PIDs
netstat -an      :: Show all connections

:: Test connectivity
ping google.com
ping -t google.com    :: Continuous ping

:: Trace route
tracert google.com

:: DNS lookup
nslookup google.com
```

```powershell
# PowerShell: Network info
Get-NetIPConfiguration
Get-NetIPAddress

# Test connection
Test-Connection google.com
Test-Connection google.com -Count 4

# DNS lookup
Resolve-DnsName google.com

# Network adapters
Get-NetAdapter
Get-NetAdapter | Select-Object Name, Status, LinkSpeed
```

### Advanced Networking
```cmd
:: Show MAC address
getmac
ipconfig /all | findstr "Physical"

:: Show routing table
route print

:: Show ARP cache
arp -a

:: Manage firewall (admin required)
netsh advfirewall show allprofiles
netsh advfirewall set allprofiles state off

:: Download file
curl https://example.com/file.zip -o file.zip
```

```powershell
# PowerShell: Download file
Invoke-WebRequest -Uri "https://example.com/file.zip" -OutFile "file.zip"
wget "https://example.com/file.zip" -OutFile "file.zip"    # Alias

# Test port connectivity
Test-NetConnection google.com -Port 443

# Get public IP
(Invoke-WebRequest -Uri "http://ifconfig.me/ip").Content
```

---

## Process Management

### Viewing Processes
```cmd
:: CMD: List processes
tasklist
tasklist | findstr chrome

:: Detailed process info
wmic process get name,processid,commandline
```

```powershell
# PowerShell: List processes
Get-Process
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10

# Find specific process
Get-Process chrome
Get-Process | Where-Object {$_.Name -like "*chrome*"}

# Process with details
Get-Process | Select-Object Name, Id, CPU, WorkingSet
```

### Managing Processes
```cmd
:: CMD: Kill process by name
taskkill /IM chrome.exe
taskkill /IM chrome.exe /F    :: Force

:: Kill process by PID
taskkill /PID 1234
taskkill /PID 1234 /F

:: Kill all instances
taskkill /IM chrome.exe /F /T    :: Including child processes
```

```powershell
# PowerShell: Stop process
Stop-Process -Name chrome
Stop-Process -Id 1234
Stop-Process -Name chrome -Force

# Start process
Start-Process notepad
Start-Process notepad -ArgumentList "C:\file.txt"
Start-Process powershell -Verb RunAs    # Run as admin
```

---

## Productivity Tips

### Command Line Shortcuts

**CMD and PowerShell Shortcuts:**
```
Ctrl + C          :: Cancel current command
Ctrl + V          :: Paste (newer Windows versions)
Ctrl + M          :: Enter Mark Mode (for copying)

Tab               :: Auto-complete file/folder names
Ctrl + Left/Right :: Jump word by word
Home / End        :: Go to start/end of line

F1                :: Paste last command character by character
F3                :: Paste last command
F7                :: Show command history (CMD)
F8                :: Search command history
F9                :: Select command from history by number

Up/Down Arrows    :: Navigate command history
Ctrl + Home       :: Delete from cursor to beginning
Ctrl + End        :: Delete from cursor to end
```

### Quick Access
```cmd
:: Open common locations
explorer .                    :: Open current folder in Explorer
start .                       :: Same as above
start %USERPROFILE%          :: Open user home folder
start %APPDATA%              :: Open AppData\Roaming
start %TEMP%                 :: Open temp folder

:: Open system apps
control                      :: Control Panel
control system               :: System Properties
devmgmt.msc                 :: Device Manager
diskmgmt.msc                :: Disk Management
services.msc                :: Services
regedit                     :: Registry Editor
msconfig                    :: System Configuration
taskmgr                     :: Task Manager
```

### Batch File Tips
```batch
@echo off
:: @ suppresses command echo
:: echo off disables command display

:: Comments start with ::

:: Variables
set myvar=value
echo %myvar%

:: User input
set /p name="Enter your name: "
echo Hello %name%

:: Conditional execution
if exist file.txt (
    echo File exists
) else (
    echo File not found
)

:: Loop through files
for %%f in (*.txt) do (
    echo %%f
)

:: Pause at end
pause
```

---

## Environment Variables

### Common Environment Variables
```cmd
:: View all environment variables
set

:: Important variables
echo %USERPROFILE%           :: C:\Users\YourName
echo %APPDATA%               :: AppData\Roaming
echo %LOCALAPPDATA%          :: AppData\Local
echo %TEMP%                  :: Temp folder
echo %PROGRAMFILES%          :: C:\Program Files
echo %PROGRAMFILES(X86)%     :: C:\Program Files (x86)
echo %SYSTEMROOT%            :: C:\Windows
echo %WINDIR%                :: C:\Windows
echo %PATH%                  :: System path

:: Date and time
echo %DATE%
echo %TIME%

:: Computer info
echo %COMPUTERNAME%
echo %USERNAME%
echo %USERDOMAIN%
```

### Setting Environment Variables
```cmd
:: CMD: Set for current session
set MYVAR=value
echo %MYVAR%

:: Set permanently (user level) - requires admin or GUI
setx MYVAR "value"

:: Add to PATH (current session)
set PATH=%PATH%;C:\new\path

:: Add to PATH permanently (requires PowerShell as admin)
```

```powershell
# PowerShell: Set for current session
$env:MYVAR = "value"
$env:MYVAR

# Set permanently (user level)
[System.Environment]::SetEnvironmentVariable('MYVAR', 'value', 'User')

# Set permanently (system level - requires admin)
[System.Environment]::SetEnvironmentVariable('MYVAR', 'value', 'Machine')

# Add to PATH (user level)
$path = [System.Environment]::GetEnvironmentVariable('PATH', 'User')
$newPath = $path + ';C:\new\path'
[System.Environment]::SetEnvironmentVariable('PATH', $newPath, 'User')
```

---

## Shortcuts and Aliases

### Creating Aliases in PowerShell
```powershell
# Create alias for current session
Set-Alias ll Get-ChildItem
Set-Alias np notepad

# Create persistent aliases (add to profile)
# First, check if profile exists
Test-Path $PROFILE

# Create profile if it doesn't exist
if (!(Test-Path $PROFILE)) {
    New-Item -Path $PROFILE -ItemType File -Force
}

# Edit profile
notepad $PROFILE

# Add to profile:
Set-Alias ll Get-ChildItem
Set-Alias np notepad

function gs { git status }
function gp { git pull }
function gc { git commit }
```

### Creating Batch File Shortcuts
```cmd
:: Create a .bat file in a PATH directory
:: For example: C:\Users\YourName\scripts\npp.bat

@echo off
"C:\Program Files\Notepad++\notepad++.exe" %*

:: Now you can type "npp file.txt" from anywhere
```

### Useful PowerShell Functions
```powershell
# Add these to your $PROFILE

# Quick directory navigation
function .. { Set-Location .. }
function ... { Set-Location ..\.. }
function .... { Set-Location ..\..\.. }

# List all files including hidden
function la { Get-ChildItem -Force }

# List only directories
function ld { Get-ChildItem -Directory }

# Open file in default program
function open { explorer $args }

# Get folder size
function Get-FolderSize {
    param([string]$path = ".")
    "{0:N2} GB" -f ((Get-ChildItem -Recurse $path | Measure-Object -Property Length -Sum).Sum / 1GB)
}

# Find file recursively
function ff {
    param([string]$name)
    Get-ChildItem -Recurse -Filter "*$name*" -ErrorAction SilentlyContinue
}
```

---

## Advanced Tricks

### Clipboard Operations
```cmd
:: CMD: Copy to clipboard
dir | clip
ipconfig | clip

:: Get clipboard content (PowerShell only)
```

```powershell
# PowerShell: Copy to clipboard
Get-Process | clip
"some text" | Set-Clipboard

# Get clipboard content
Get-Clipboard
```

### Working with JSON/XML
```powershell
# PowerShell: Parse JSON
$json = Get-Content data.json | ConvertFrom-Json
$json.propertyName

# Create JSON
$object = @{
    name = "John"
    age = 30
}
$object | ConvertTo-Json | Out-File output.json

# Parse XML
[xml]$xml = Get-Content data.xml
$xml.root.element
```

### Scheduled Tasks
```cmd
:: CMD: Create scheduled task
schtasks /create /tn "TaskName" /tr "C:\script.bat" /sc daily /st 09:00

:: List scheduled tasks
schtasks /query

:: Run a task
schtasks /run /tn "TaskName"

:: Delete a task
schtasks /delete /tn "TaskName"
```

```powershell
# PowerShell: Scheduled tasks
Get-ScheduledTask
Get-ScheduledTask -TaskName "TaskName"

# Create new task
$action = New-ScheduledTaskAction -Execute "C:\script.bat"
$trigger = New-ScheduledTaskTrigger -Daily -At 9am
Register-ScheduledTask -TaskName "TaskName" -Action $action -Trigger $trigger
```

### System Administration
```cmd
:: Check system uptime (requires admin)
systeminfo | findstr "System Boot Time"

:: List installed programs
wmic product get name,version

:: Check battery status (laptops)
wmic path Win32_Battery get EstimatedChargeRemaining

:: Shutdown/Restart
shutdown /s /t 0     :: Shutdown now
shutdown /r /t 0     :: Restart now
shutdown /r /t 3600  :: Restart in 1 hour
shutdown /a          :: Abort shutdown
```

```powershell
# PowerShell: System admin
Get-Uptime

# Get installed programs
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* |
    Select-Object DisplayName, DisplayVersion, Publisher

# Restart computer
Restart-Computer
Restart-Computer -Force

# Stop computer
Stop-Computer
```

---

## Windows Subsystem for Linux (WSL)

### WSL Basics
```powershell
# Install WSL (requires admin)
wsl --install

# List available distributions
wsl --list --online

# Install specific distribution
wsl --install -d Ubuntu-22.04

# List installed distributions
wsl --list --verbose
wsl -l -v

# Set default distribution
wsl --set-default Ubuntu-22.04

# Update WSL
wsl --update
```

### Using WSL
```cmd
:: Start default WSL distribution
wsl

:: Run Linux command from Windows
wsl ls -la
wsl grep -r "pattern" /home/user/

:: Access Windows files from WSL
:: /mnt/c/ = C:\
wsl ls /mnt/c/Users

:: Access WSL files from Windows
:: \\wsl$\Ubuntu\home\user\
explorer \\wsl$\Ubuntu\home\user\
```

### WSL Integration
```powershell
# Run Windows programs from WSL
# (from within WSL)
notepad.exe file.txt
explorer.exe .

# Share clipboard between Windows and WSL
# Works automatically in WSL 2

# Shutdown WSL
wsl --shutdown

# Terminate specific distribution
wsl --terminate Ubuntu-22.04
```

---

## Quick Reference

### Most Useful Commands
```cmd
:: File operations
dir, cd, mkdir, del, copy, move, xcopy

:: System info
systeminfo, hostname, whoami, ipconfig

:: Network
ping, tracert, nslookup, netstat

:: Process management
tasklist, taskkill

:: Utilities
cls, find, findstr, where, clip
```

### PowerShell Essentials
```powershell
# Get help
Get-Help, Get-Command, Get-Member

# File operations
Get-ChildItem, Set-Location, Copy-Item, Remove-Item

# System
Get-Process, Get-Service, Get-ComputerInfo

# Network
Test-Connection, Test-NetConnection, Invoke-WebRequest
```

### One-Liners for Common Tasks
```cmd
:: Find large files
forfiles /S /C "cmd /c if @fsize gtr 100000000 echo @path @fsize"

:: List files modified today
forfiles /P C:\path /D +0

:: Count files in directory
dir /b | find /c /v ""
```

```powershell
# Find large files (over 100MB)
Get-ChildItem -Recurse | Where-Object {$_.Length -gt 100MB} | Select-Object FullName, Length

# Find old files (over 30 days)
Get-ChildItem -Recurse | Where-Object {$_.LastWriteTime -lt (Get-Date).AddDays(-30)}

# Count files
(Get-ChildItem -Recurse -File).Count

# Free up disk space
Clear-RecycleBin -Force
```

---

## Tips for Better Productivity

1. **Use Tab Completion**: Press Tab to auto-complete file and folder names
2. **Create Aliases**: Set up shortcuts for frequently used commands
3. **Learn PowerShell**: More powerful and modern than CMD
4. **Use WSL**: Get Linux tools on Windows
5. **Customize Your Profile**: Add functions and aliases to PowerShell $PROFILE
6. **Learn Keyboard Shortcuts**: Speed up navigation and editing
7. **Use Windows Terminal**: Modern, tabbed terminal with better features
8. **Script Repetitive Tasks**: Create batch or PowerShell scripts
9. **Master Piping**: Chain commands together with `|`
10. **Use Clipboard**: Send output to clipboard with `| clip`

---

## Setting Up Windows Terminal (Recommended)

Windows Terminal is a modern, feature-rich terminal application.

### Install Windows Terminal
```powershell
# From Microsoft Store: Search "Windows Terminal"
# Or via winget:
winget install Microsoft.WindowsTerminal
```

### Useful Features
- **Multiple tabs**: Ctrl+Shift+T (new tab), Ctrl+W (close tab)
- **Split panes**: Alt+Shift+D (auto split), Alt+Shift+- (horizontal), Alt+Shift++ (vertical)
- **Multiple profiles**: PowerShell, CMD, WSL, Azure Cloud Shell
- **Customizable**: Colors, fonts, keybindings
- **Copy/Paste**: Ctrl+C, Ctrl+V work natively

### Quick Settings
- Open settings: Ctrl+,
- Settings file: JSON-based configuration
- Themes and color schemes available
- Can set default profile (PowerShell, CMD, or WSL)
