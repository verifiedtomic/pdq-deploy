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

**You must restart the PC after the package is complete so that printer can become active**
