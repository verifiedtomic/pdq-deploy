# pdq-deploy

## Create Environment Variable

This package creates system environment variable in Windows OS.

**You must restart the PC after the package is complete, because OS loads environment variables on startup.**

## Default Power Settings

This package sets Balanced Power Plan in Control Panel Power Options to custom settings:

Action | On battery | Plugged in
------------ | ------------- | -------------
Turn off the display | 45 minutes | 1 hour
Put the computer to sleep | 1 hour | Never
Low battery | 10% | 10%
Critical battery | 3% | 3%

You can create your own power plan on [Windows AFG site](https://www.windowsafg.com/power10.html), save the commands in .bat file and replace my PowerSettings.bat with yours in the Install step.

**Important option is to run this package as Logged on user, because power options are custom set for each individual machine user.**

You can also use this package settings to run other .bat files!

## Install Network Printer

This package disables *Let Windows manage default printer*, installs the network printer to the PC, and set's it to be the default printer via PowerShell.

**Important option is to run this package as Logged on user, because this PS script installs the printer to current user.**

**You must restart the PC after the package is complete so that printer can become active.**

## Install Teams Meetings Backgrounds

This package copies custom background images from source to logged-on user AppData folder.

**Important option is to run this package as Logged on user, because Teams pulls backgrounds for every user from their AppData folder.**

**You must restart the Teams app after the package is complete because Teams loads backgrounds from folder on startup.**

## MSFT Deployment Toolkit (MDT)

This isn't PDQ Deploy package, but if you are using PDQ you probably don't have SCCM.
Microsoft Deployment Toolkit is enables you to deploy default and custom Windows images over network and PXE boot.
MDT is a standalone feature that can be enabled on Windows Server machine or [installed on any Windows 10](https://www.microsoft.com/en-us/download/details.aspx?id=54259) machine that will act as deployment server.

I suggest reading [MDT documentation](https://docs.microsoft.com/en-us/mem/configmgr/mdt/?redirectedfrom=MSDN) before configurin MDT in your environment.

In this repository's folder you'll find three most important files for unattanded Windows installation:
- Bootstrap.ini
- CustomSetings.ini
- Unattend.xml

Since .ini files don't work when they have inline comments in them, I have listed switches I used in my implementation and their explanations. Unattend.xml file has inline comment where you have to change or input some custom settings.

#### Bootstrap.ini

Switch | Explanation
------------ | -------------
TaskSequenceID | In my implementation I have used single default Task Sequence. I you use single TS, type TS's ID as this switch's value
DeployRoot | UNC path to DeploymentShare folder
UserDataLocation | Used for current user's data backup. I didn't use this
UserDomain | Service account's used for accessing Deployment Share domain. Your domain
UserID | Service account to access Deployment Share
UserPassword | Service accounts password
OSInstall | Install OS. YES, of course
UserLocale | [Here type chosen language code for Region](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-7/dd744369(v=ws.10))
UILanguage | [Here type chosen language code for UI Language](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-7/dd744369(v=ws.10))
KeyboardLocale | [Here type chosen language code for default system keyboard](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/default-input-locales-for-windows-language-packs)
KeyboardLocalePE | [Here type chosen language code for default system keyboard in Windows PE](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/default-input-locales-for-windows-language-packs)
DomainAdmin | Service account to add computer do domain
DomainAdminDomain | Service account's domain. Your domain
DomainAdminPassword | Service account's password
JoinDomain | FQDN of domain
TimeZone | [Here type chosen time zone code for computer's time](https://ss64.com/nt/timezones.html)
TimeZoneName | Symbolic name of chosen time zone
SkipBDDWelcome | Skip MDT Welcome screen
_SMSTSOrgName | String that will be shown during TS execution

#### CustomSettings.ini

Switch | Explanation
------------ | -------------
TaskSequenceID | In my implementation I have used single default Task Sequence. I you use single TS, type TS's ID as this switch's value
UserDataLocation | Used for current user's data backup. I didn't use this
UserDomain | Service account's used for accessing Deployment Share domain. Your domain
UserID | Service account to access Deployment Share
UserPassword | Service accounts password
OSInstall | Install OS. YES, of course
UserLocale | [Here type chosen language code for Region](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-7/dd744369(v=ws.10))
UILanguage | [Here type chosen language code for UI Language](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-7/dd744369(v=ws.10))
KeyboardLocale | [Here type chosen language code for default system keyboard](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/default-input-locales-for-windows-language-packs)
KeyboardLocalePE | [Here type chosen language code for default system keyboard in Windows PE](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/default-input-locales-for-windows-language-packs)
DomainAdmin | Service account to add computer do domain
DomainAdminDomain | Service account's domain. Your domain
DomainAdminPassword | Service account's password
JoinDomain | FQDN of domain
DomainOUsN | Canonical path of Active Direcotry OU's where you store computer accounts. List OU which you want to be shown in dropdown to add computer in
TimeZone | [Here type chosen time zone code for computer's time](https://ss64.com/nt/timezones.html)
TimeZoneName | Symbolic name of chosen time zone
SkipBDDWelcome | Skip MDT Welcome screen
SkipApplications | Skip application installation. You don't need this if you don't have SCCM configured
SkipSummary | Skip deployment summary
SkipUserData | Skip restoring user data
SkipComputerName | Skip wizard where you type computer's name and choose OU
SkipDomainMembership | Skip wizard where you add computer to domain
SkipTaskSequence | Skip TS wizard. YES if you use single default TS
SkipLocaleSelection | Skip Region and UI language selection.
SkipTimeZone | Skip Time Zone Selection
SkipAppsOnUpgrade | Skip application upgrade. You don't need this if you don't have SCCM configured
SkipCapture | Skip creating current system image from connected computer
SkipAdminPassword | Skip setting admin password for default local admin user
SkipProductKey | Skip prompt for entering Windows Serial Key
SkipBitLocker | Skip BitLocker activation
_SMSTSOrgName | String that will be shown during TS execution
SkipFinalSummary | Skip deployment summary
SkipComputerBackup | Skip file backup from connected computer
ComputerBackupLocaton | Location of file backup, if you don't skip computer backup
SkipDeploymentType | Skip Deployment type selection. You don't need this if you will user MDT for inital OS installs, and re-installs
DeploymentType | MDT have multiple Deployment types. Different processes are executed based on selected DeploymentType. NEWCOMPUTER is for initial OS installs and re-installs
EventService | URL to monitoring website. You have to enable monitoring in MDT Console.

**You probably noticed that some switches are used in both Bootstrap.ini and CustomSettings.ini. Bootstrap.ini and CustomSettings.ini are interchangeable. Difference between the two is that for every change to Bootstrap.ini you have to rebuild DeploymentShare, and settings are saved in boot.wim file. Computer reads CustomSettings.ini on every PXE boot, which means that you can dynamically change settings between computers, or test changes. In other words, settings saved in Bootstrap.ini are _permanent_ and changes saved in CustomSettings.ini are _dynamic_**