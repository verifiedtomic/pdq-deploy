# pdq-deploy

**DefaultPowerSettings.xml**

This package sets Balanced Power Plan in Control Panel Power Options to custom settings:

Action | On battery | Plugged in
------------ | ------------- | -------------
Turn off the display | 45 minutes | 1 hour
Put the computer to sleep | 1 hour | Never
Low battery | 10% | 10%
Critical batter | 3% | 3%

You can create your own power plan on [Windows AFG site](https://www.windowsafg.com/power10.html), save the commands in .bat file and replace my PowerSettings.bat with yours in the Install step.

**Important option is to run this package as Logged on user, because power options are custom set for each individual machine user.**

You can also use this package settings to run other .bat files!