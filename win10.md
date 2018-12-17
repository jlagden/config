**Useful Links**

* https://docs.microsoft.com/en-gb/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services
* https://getadmx.com/
* https://www.windows-security.org

**Windows Settings**

* **System** 
  * **Notifications and Actions**
    * **Notifications** 
      * Show me the Windows welcome experience after updates and occasionally when I sign in to highlight what's new and suggested **OFF**
      * Get tips, tricks and suggestions as you use Windows **OFF**
  * **Multitasking**
    * **Timeline**
      * Show suggestions occasionally in Timeline **OFF**
  * **Shared experiences**
    * **Share across devices**
      * Let apps on other devices (including linked phones and tablets) open and message apps on this device, and vice versa **OFF**

Devices -> Bluetooth amd other

* bluetooth

Devices -> Typing

Spelling

* Auto correct mispelt words
* highlight mispelt words

Typing

* Show text suggestions as I type on software keyboard
* Add a space after I choose a text suggestion
* add a full stop after I double tap the spacebar

Devices -> autoplay

* autoplay

Network & Internet -> Wifi

* Let me use Online Sign-up to get connected

Network & Internet -> Mobile Hotspot

* Turn on REmotely

Settings -> Start

* Show the recently added apps
* Show suggestions occasionally in Start
* Show recently opened items in Jump List on start or the taskbar

Settings -> Taskbar -> People

* Show contacts on the taskbar

Apps -> Startup

*Onedrive

Settings -> Gaming -> Gamebar

Off

Settings -> Gaming -> Game Mode

Off

Settings -> Cortana -> Talk to Cortanca

* USe Cortana even when my device is locked

Settings -> Permissions & History -> SafeSearch

* Off

Settings -> Permissions & History

* Windows Cloud Search

Settings -> Permisssions & History

* View activity history
* Activity recommendations
* My device history

Settings -> Privacy -> General

* Allow websites to provide locally relevant content by accessing my language list
* Allow Windows to track app launches to improve startup and search results
* Show me suggested content in the the settings app

Settings -> Privacy -> Inking and typing

OFF

Settings -> Privacy -> Diagnostics & Feedback

* Feedback frequency - NEVER

Settings - Privacy -> Activity History

* Store my activity history on this device

CLEAR HISTORY

Settings -> Privacy -> Account info

* Allow apps to access your account info

Settings -> Privacy -> Contacts

* Allow apps to access your contacts

Settings -> Privacy -> Calendar

* Allow apps to access your calendar

Settings -> Privacy -> Call history

* Allow apps to acces your call history

Settings -> Privacy -> Email

* Allow apps to access your email

Settings -> Privacy -> Tasks

* Allow apps to access your tasks

Settings -> Privacy -> Messenging

* Allow apps to read or send messages

Settings -> Privacy -> Radios

* Allow apps to control radios

Settings -> Privacy -> Other devices

* Communicate with unpaired devices

Settings -> Privacy -> App Diagnostics

* App diagnostic info access for device

Settings -> Privacy -> Documents

* Allow apps to access your documents library

Settings -> Privacy -> Pictures

* Picture library access for this device

Settings -> Privacy -> Videos

* Video Library access for this device

Settings -> Privacy -> File System

* File system access for this device

Settings -> Update & Security -> Delivery Optimisation

* Allow downloads from other PCs

Settings -> Update & Security -> For Developers 

File Explorer - APPLY


install_wim_tweak /o /c Microsoft-Windows-ContentDeliveryManager /r
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\AppHost" /v "EnableWebContentEvaluation" /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\PushToInstall" /v DisablePushToInstall /t REG_DWORD /d 1 /f
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SilentInstalledAppsEnabled /t REG_DWORD /d 0 /f
sc delete PushToInstall

Get-AppxPackage -AllUsers *zune* | Remove-AppxPackage
Get-AppxPackage -AllUsers *xbox* | Remove-AppxPackage

```
sc delete XblAuthManager
sc delete XblGameSave
sc delete XboxNetApiSvc
sc delete XboxGipSvc
reg delete "HKLM\SYSTEM\CurrentControlSet\Services\xbgm" /f
schtasks /Change /TN "Microsoft\XblGameSave\XblGameSaveTask" /disable
schtasks /Change /TN "Microsoft\XblGameSave\XblGameSaveTaskLogon" /disable
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\GameDVR" /v AllowGameDVR /t REG_DWORD /d 0 /f
```

Get-AppxPackage -AllUsers *sticky* | Remove-AppxPackage

Get-AppxPackage -AllUsers *maps* | Remove-AppxPackage

sc delete MapsBroker
sc delete lfsvc
schtasks /Change /TN "\Microsoft\Windows\Maps\MapsUpdateTask" /disable

Get-AppxPackage -AllUsers *alarms* | Remove-AppxPackage
Get-AppxPackage -AllUsers *people* | Remove-AppxPackage

Get-AppxPackage -AllUsers *comm* | Remove-AppxPackage
Get-AppxPackage -AllUsers *mess* | Remove-AppxPackage

Get-AppxPackage -AllUsers *onenote* | Remove-AppxPackage
Get-AppxPackage -AllUsers *photo* | Remove-AppxPackage
Get-AppxPackage -AllUsers *camera* | Remove-AppxPackage
Get-AppxPackage -AllUsers *bing* | Remove-AppxPackage
Get-AppxPackage -AllUsers *calc* | Remove-AppxPackage
Get-AppxPackage -AllUsers *soundrec* | Remove-AppxPackage

install_wim_tweak /o /c Microsoft-Windows-Holographic /r
Reboot (important!) and reopen our command prompt and PowerShell
Get-AppxPackage -AllUsers *holo* | Remove-AppxPackage
Get-AppxPackage -AllUsers *3db* | Remove-AppxPackage
Get-AppxPackage -AllUsers *3dv* | Remove-AppxPackage
Get-AppxPackage -AllUsers *paint* | Remove-AppxPackage
Get-AppxPackage -AllUsers *mixed* | Remove-AppxPackage
Get-AppxPackage -AllUsers *print3d* | Remove-AppxPackage
for /f "tokens=1* delims=" %I in (' reg query "HKEY_CLASSES_ROOT\SystemFileAssociations" /s /k /f "3D Edit" ^| find /i "3D Edit" ') do (reg delete "%I" /f )
for /f "tokens=1* delims=" %I in (' reg query "HKEY_CLASSES_ROOT\SystemFileAssociations" /s /k /f "3D Print" ^| find /i "3D Print" ') do (reg delete "%I" /f )

install_wim_tweak /o /c Microsoft-Windows-ContactSupport /r
Get-AppxPackage -AllUsers *GetHelp* | Remove-AppxPackage
Additionally, Go to Start > Settings > Apps > Manage optional features, and remove Contact Support (if present).

Microsoft Quick Assist
Go to Start > Settings > Apps > Manage optional features, and remove Microsoft Quick Assist
Connect
install_wim_tweak /o /c Microsoft-PPIProjection-Package /r

Your Phone
Get-AppxPackage -AllUsers *phone* | Remove-AppxPackage
Snip & Sketch
Get-AppxPackage -AllUsers *sketch* | Remove-AppxPackage
Hello Face
Go to Start > Settings > Apps > Manage optional features, and remove Hello Face.
schtasks /Change /TN "\Microsoft\Windows\HelloFace\FODCleanupTask" /Disable

reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\Windows Search" /v AllowCortana /t REG_DWORD /d 0 /f

reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\Windows Error Reporting" /v Disabled /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting" /v Disabled /t REG_DWORD /d 1 /f

Disable sync
It doesn't really affect you if you're not using a Microsoft Account, but it will at least disable the Sync settings from the Settings app
reg add "HKLM\Software\Policies\Microsoft\Windows\SettingSync" /v DisableSettingSync /t REG_DWORD /d 2 /f
reg add "HKLM\Software\Policies\Microsoft\Windows\SettingSync" /v DisableSettingSyncUserOverride /t REG_DWORD /d 1 /f
No Windows Tips
reg add "HKLM\Software\Policies\Microsoft\Windows\CloudContent" /v DisableSoftLanding /t REG_DWORD /d 1 /f
reg add "HKLM\Software\Policies\Microsoft\Windows\CloudContent" /v DisableWindowsSpotlightFeatures /t REG_DWORD /d 1 /f
reg add "HKLM\Software\Policies\Microsoft\Windows\CloudContent" /v DisableWindowsConsumerFeatures /t REG_DWORD /d 1 /f
reg add "HKLM\Software\Policies\Microsoft\Windows\DataCollection" /v DoNotShowFeedbackNotifications /t REG_DWORD /d 1 /f
reg add "HKLM\Software\Policies\Microsoft\WindowsInkWorkspace" /v AllowSuggestedAppsInWindowsInkWorkspace /t REG_DWORD /d 0 /f

Removing OneDrive
If you don't use OneDrive (and you shouldn't), you can remove it from your system with these commands, entered in the command prompt: 
taskkill /F /IM onedrive.exe
If you're on 32 bit Windows:
"%SYSTEMROOT%\System32\OneDriveSetup.exe" /uninstall
If you're on 64 bit Windows:
"%SYSTEMROOT%\SysWOW64\OneDriveSetup.exe" /uninstall
In the command prompt type:
rd "%USERPROFILE%\OneDrive" /Q /S
rd "C:\OneDriveTemp" /Q /S
rd "%LOCALAPPDATA%\Microsoft\OneDrive" /Q /S
rd "%PROGRAMDATA%\Microsoft OneDrive" /Q /S
reg delete "HKEY_CLASSES_ROOT\CLSID\{018D5C66-4533-4307-9B53-224DE2ED1FE6}" /f
reg delete "HKEY_CLASSES_ROOT\Wow6432Node\CLSID\{018D5C66-4533-4307-9B53-224DE2ED1FE6}" /f
del /Q /F "%localappdata%\Microsoft\OneDrive\OneDriveStandaloneUpdater.exe"

Removing Telemetry and other unnecessary services
In the command prompt, type the following commands: 

```
sc delete DiagTrack
sc delete dmwappushservice
sc delete WerSvc
sc delete OneSyncSvc
sc delete MessagingService
sc delete wercplsupport
sc delete PcaSvc
sc config wlidsvc start=demand
sc delete wisvc
sc delete RetailDemo
sc delete diagsvc
sc delete shpamsvc

for /f "tokens=1" %I in ('reg query "HKLM\SYSTEM\CurrentControlSet\Services" /k /f "OneSyncSvc" ^| find /i "OneSyncSvc"') do (reg delete %I /f)
for /f "tokens=1" %I in ('reg query "HKLM\SYSTEM\CurrentControlSet\Services" /k /f "MessagingService" ^| find /i "MessagingService"') do (reg delete %I /f)
for /f "tokens=1" %I in ('reg query "HKLM\SYSTEM\CurrentControlSet\Services" /k /f "PimIndexMaintenanceSvc" ^| find /i "PimIndexMaintenanceSvc"') do (reg delete %I /f)
for /f "tokens=1" %I in ('reg query "HKLM\SYSTEM\CurrentControlSet\Services" /k /f "UserDataSvc" ^| find /i "UserDataSvc"') do (reg delete %I /f)
for /f "tokens=1" %I in ('reg query "HKLM\SYSTEM\CurrentControlSet\Services" /k /f "UnistoreSvc" ^| find /i "UnistoreSvc"') do (reg delete %I /f)
for /f "tokens=1" %I in ('reg query "HKLM\SYSTEM\CurrentControlSet\Services" /k /f "BcastDVRUserService" ^| find /i "BcastDVRUserService"') do (reg delete %I /f)

sc delete diagnosticshub.standardcollector.service

reg add "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Siuf\Rules" /v "NumberOfSIUFInPeriod" /t REG_DWORD /d 0 /f
reg delete "HKEY_CURRENT_USER\SOFTWARE\Microsoft\Siuf\Rules" /v "PeriodInNanoSeconds" /f
reg add "HKLM\SYSTEM\ControlSet001\Control\WMI\AutoLogger\AutoLogger-Diagtrack-Listener" /v Start /t REG_DWORD /d 0 /f
```

**Turn off Application Telementry** [Link](https://www.windows-security.org/13372c6a0a2d392443dc146ceb94d720/turn-off-application-telemetry)

reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\AppCompat" /v AITEnable /t REG_DWORD /d 0 /f

**Turn off Inventory Collector** [Link](https://www.windows-security.org/8cb262cbd4b94fb7d3ec810760e83587/turn-off-inventory-collector)

`reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\AppCompat" /v DisableInventory /t REG_DWORD /d 1 /f`

**Disable Program Compatibility Assistant**

`reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\AppCompat" /v DisablePCA /t REG_DWORD /d 1 /f`

**Turn off steps recorder**

`reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\AppCompat" /v DisableUAR /t REG_DWORD /d 1 /f`

**Turn off windows defender smart screen**

`reg add "HKLM\SOFTWARE\Policies\Microsoft\MicrosoftEdge\PhishingFilter" /v "EnabledV9" /t REG_DWORD /d 0 /f` 

**Turn off smart screen for all users**

`reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\System" /v "EnableSmartScreen" /t REG_DWORD /d 0 /f` 

Create a REG_DWORD registry setting named EnableSmartScreen in HKEY_LOCAL_MACHINE\Sofware\Policies\Microsoft\Windows\System with a value of 0 (zero).
-and-
Create a REG_DWORD registry setting named ConfigureAppInstallControlEnabled in HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender\SmartScreen with a value of 1.
-and-
Create a SZ registry setting named ConfigureAppInstallControl in HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender\SmartScreen with a value of Anywhere.

**Turn off Internet Explorer smart screen filter**

`reg add "HKCU\Software\Microsoft\Internet Explorer\PhishingFilter" /v "EnabledV9" /t REG_DWORD /d 0 /f` 

**Disable recent documents history**

`reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v "NoRecentDocsHistory" /t REG_DWORD /d 1 /f`
