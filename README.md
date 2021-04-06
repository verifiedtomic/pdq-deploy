# pdq-deploy

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

## Create Environment Variable

This package creates system environment variable in Windows OS.

**You must restart the PC after the package is complete, because OS loads environment variables on startup.**
