---
description: Did you plug the device in?
---

# 🔌 v0.0.7.x series

<figure><img src="https://officialaptivi.files.wordpress.com/2022/02/68747470733a2f2f692e696d6775722e636f6d2f776870656135482e706e67.png" alt=""><figcaption></figcaption></figure>

This series marks the moment when the documentation moved to GitHub Wiki, which was an advancement for the documentation. However, this release wasn't as big as all the major first-gen releases.

Out of all the version series that provided minor updates, this series is the only one that its first digit in the last version section denotes the feature pack and its second digit informs you about how many patches applied. This was because semantic versioning limits the version parts to four parts.

For beta versions of this series, consult the below link.

{% content-ref url="v0.0.7.0-beta-versions.md" %}
[v0.0.7.0-beta-versions.md](v0.0.7.0-beta-versions.md)
{% endcontent-ref %}

### Version 0.0.7.0

This version was released on August 30, 2019 to remove the manual page viewer from the kernel and add FTPS.

#### Additions

* Added FTPS

#### Improvements

* Fixed listing directories that have spaces in them
* General improvements

#### Removals

* Removed network pinging
* Removed listing computers
* Removed manual page viewer

### Version 0.0.7.1

This version was named to be the first feature pack for the series, which was released on August 31, 2019.

#### Additions

* Added the Indonesian, Polish, Romanian, and Uzbekistan languages
* Added remote debugging
* Added the get command to download URLs

#### Improvements

* Configuration improvements regarding colors
* Updated the FTP library, FluentFTP

#### Removals

* Removed the useddeps command

### Version 0.0.7.11

This version, which was released on September 3, 2019, was the first patch for the first feature pack.

#### Improvements

* Argument injector checks for arguments
* showtdzone can now show time in a specific time zone
* Added several error handlers to several commands, including chdir and chpwd
* netinfo looks tidier
* Various fixes

### Version 0.0.7.12

This version was out on September 5, 2019.

#### Additions

* Customizability of the remote debug port and download retry count

#### Improvements

* Fixed get not downloading anything containing the URL query strings

### Version 0.0.7.13

This was released on September 15, 2019, to add the built-in chat feature for remote debugger.

#### Additions

* Added the built-in chat on remote debugger console

#### Improvements

* Fixed invalid arguments returning wrong exception
* Improved the quiet kernel system

### Version 0.0.7.14

This version was out on September 21, 2019.

#### Improvements

* Fixed defect in the remote debug chat system causing chats to take turns

### Version 0.0.7.2

This version was named as the second feature release for this series, and it was released on September 23, 2019.

#### Additions

* Added Yellow background (YellowBG) and foreground (YellowFG) themes
* Added the Japanese language

#### Improvements

* Updated the FTP library, FluentFTP
* Time zone offsets are now shown
* Fixed several graphics artifacts regarding light backgrounds and time/date corner widget

### Version 0.0.7.21

This version was out on September 29, 2019 to make improvements.

#### Improvements

* Fixed Japanese language not keeping up with the latest strings
* Fixed FTP connection routine not prompting the user for profile selection
* General improvements

### Version 0.0.7.3

This version was released on October 4, 2019 to bring the third feature package to the series.

#### Additions

* Added command support to remote debugger
* Added Arabic transliteration

#### Improvements

* Fixed empty address being accepted in the connect FTP command
* Fixed crash when trying to handle non-socket problems

### Version 0.0.7.4

This version started the fourth feature package for this series, which appeared on October 18, 2019.

#### Additions

* Added kernel test shell
* Added debug quota

#### Improvements

* Fixed debugger not flushing info to the debug file after clearing debug log
* Updated the FTP library, FluentFTP
* Improved NuGet packaging

### Version 0.0.7.41

On October 19, 2019, this patch got released.

#### Improvements

* Fixed progress bar causing color bleed (using default console color)
* Clarified the ETA for FTP file transfer

### Version 0.0.7.5

This is the fifth feature update to the 0.0.7.x series that got released on October 24, 2019.

#### Additions

* Argument support to the remote debug commands is now added
* Added the username debug command
* Added the stack trace viewer trace

#### Improvements

* Two newly-added shells now complain when the specified command wasn't found
* Fixed stack trace history not getting updated in some cases

### Version 0.0.7.6

This version was out on October 29, 2019 as a last feature update for this series.

#### Additions

* Added naming system to the remote debugger chat (generated names only)
* Added the lines and glitterMatrix screensavers

#### Improvements

* Extended debugging to more areas of the kernel

### Version 0.0.7.61

This was the last version of the kernel to declare itself as API v1.0. It got released on October 30, 2019 to bring several fixes.

#### Additions

* Added the transliterated Russian language

#### Improvements

* Fixed crash on startup in modded kernels
* Fixed translations
* Custom screensavers can now cause console cursor to disappear
