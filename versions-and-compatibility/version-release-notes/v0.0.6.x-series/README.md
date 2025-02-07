---
description: The book is done!
---

# 📕 v0.0.6.x series

The whole series book was done and it was published with the release of 0.0.6.0 on June 19, 2019. This series was known to have started the real documentation of the kernel which evolved to this very documentation.

For beta versions of this series, consult the below link.

{% content-ref url="v0.0.6.0-beta-versions.md" %}
[v0.0.6.0-beta-versions.md](v0.0.6.0-beta-versions.md)
{% endcontent-ref %}

### Version 0.0.6.0

Nitrocid KS 0.0.6.0 was out on June 19, 2019 to finalize the initial version of the series with bug fixes. It was also the first version to have renewed the icon for the first time.

#### Additions

* Permanent aliases
* Added more languages such as Dutch
* Hidden passwords for more security

#### Improvements

* Updated dependencies
* Log-in rewritten
* Fixed several problems with adduser
* Fixed reboot not clearing the screen
* Several improvements were made

#### Removals

* cdir command

### Version 0.0.6.1

This version existed solely to prompt every user who needed to use currency converter for the CurrencyLayer API key, which was incorporated with the free currency API around June 2019. This version was out on June 21, 2019.

#### Improvements

* Users are now required to enter their API Key from apilayer.net to convert currencies

#### Removals

* Removed currency information from the currency converter help command entry

### Version 0.0.6.2

0.0.6.2 was released on June 24, 2019, to add modding using C# codefiles for the first time to the kernel. This version was named for being the first sign of slowly bringing C# support to Nitrocid KS, to eventually being C# only in API v3.0 kernels and up.

It was out in several variants: the original version got released at June 24, 2019, the 0.0.6.2a version at the same date, and 0.0.6.2b version at June 25, 2019.

#### Additions

* C# modding
* A warning about using PC listing commands for Windows 10/11 users
* Processor instructions (0.0.6.2a)
* sses command to list what SSE versions your processor currently holds (0.0.6.2b)

#### Improvements

* Improvements regarding the debug system

### Version 0.0.6.3

This update was released on June 26, 2019.

#### Additions

* Czech language
* A command to change language

#### Improvements

* Messages should appear again after logging in
* Quiet mode is now really quiet
* Help text shouldn't show up after executes the sses command

### Version 0.0.6.4

This version was out on June 28, 2019. It came in three variants: the original one, the 0.0.6.4a which was released on July 6, 2019, and the 0.0.6.4b which was released on July 7 of the same year.

#### Additions

* Ubuntu theme

#### Improvements

* Fixed documentation system printing a lot of new lines
* Fixed failure when trying to change the language
* Fixed Linux hardware probing failed even if it succeeded (0.0.6.4a)
* Download debug symbols upon startup (0.0.6.4b)
* Other assorted improvements

### Version 0.0.6.5

This series has put the massive changes in each 0.0.x.5 version to stop, because this version didn't have many changes and additions. It was released on July 25, 2019.

#### Additions

* Added network progress ETA and speed

#### Improvements

* Fixed bugs affecting the filesystem module
* Updated languages
* Localized manual pages

### Version 0.0.6.6

This version marked the end of currency converters. It took effect on July 26, 2019 with the release of this version.

We now know that the removal was reasonable following the recent inflation, as well as the inaccuracy of the currency API providers.

#### Improvements

* Updated the kernel manual page to add new commands

#### Removals

* Removed the currency converter

## Known bugs

Initial documentation for these versions have mentioned the following known bugs in the "`troubleshooting`" document: _(original wording preserved for brevity - only covers pre-2021 releases)_

<table><thead><tr><th width="111">Found</th><th width="93">Fixed</th><th>Description</th></tr></thead><tbody><tr><td>0.0.6.x</td><td>0.0.7.0</td><td><ol><li>The viewer is not displayed correctly</li><li>Some words might print on the last line where it shows you information on the manual page viewer</li></ol></td></tr></tbody></table>
