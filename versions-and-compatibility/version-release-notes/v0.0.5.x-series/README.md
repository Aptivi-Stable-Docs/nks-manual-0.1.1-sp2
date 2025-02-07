---
description: The time has come!
---

# 🕔 v0.0.5.x series

The time has come to bring the series to the stable release. It added aliases, permanent color setting, and so on.

For beta versions of this series, consult the below link.

{% content-ref url="v0.0.5.0-beta-versions.md" %}
[v0.0.5.0-beta-versions.md](v0.0.5.0-beta-versions.md)
{% endcontent-ref %}

### Version 0.0.5.0

On September 4, 2018, this version was made to start the prompt obsoletion movement and make several improvements to the beta versions of 0.0.5.

#### Additions

* User-made commands in mods
* Added the showaliases command
* Permanent color setting

#### Improvements

* Made several fixes, including the alias parser
* Beep command prevents frequencies more than 2048 Hz
* Removing aliases now no longer requires a command to be provided
* General improvements

#### Removals

* Removed prompts
* Removed motd, chkn=1, preadduser and hostname arguments

### Version 0.0.5.1

This release only contains improvements to debugging system regarding an area of the kernel. It got released on September 6, 2018.

#### Improvements

* Improved behavior of debugging logs
* Improved a debug message
* Made changes to startup banner

### Version 0.0.5.2

It got released on September 9, 2018, to make changes to graphics card parsing.

#### Improvements

* Made graphics card parsing always on startup
* Improved config updater

#### Removals

* Removed the gpuprobe argument

### Version 0.0.5.5

On September 22, 2018, it made a major rewrite of the kernel configuration, as well as some additions and improvements. This was the last version that was unsupported on Unix systems.

#### Additions

* Added the FTP client
* Added more text placeholders, such as shortdate, longdate, etc.

#### Improvements

* Configuration system refined
* Fixed repeating memory messages
* General improvements

### Version 0.0.5.6

This was the first version that ran in all Unix systems using the Mono Runtime framework. It was out on October 12, 2018.

#### Additions

* Support for Linux systems

#### Improvements

* Improved hardware probers
* Username list on login
* help arginj now lists improvements
* Other improvements...

### Version 0.0.5.7

On October 13, 2018, several improvements to the kernel regarding Unix support surfaced.

#### Additions

* Print current directory in FTP shell

#### Improvements

* Fixed crash if the executable name is something other than "Kernel Simulator.exe"
* Fixed "Quiet Probe" configuration entry being set to some strange value
* Fixed a known bug affecting Unix systems

#### Removals

* Version command

### Version 0.0.5.8

This version was released on November 1, 2018, to make several improvements to the kernel, but withdraws some features.

This release was declared to be the last release supporting Windows XP systems with .NET Framework 4.5 installed.

#### Improvements

* Even if one of the hardware failed to parse, the kernel continues to boot
* Only administrators can access reloadconfig and alias commands

#### Removals

* The kernel no longer beeps when rebooting and shutting down
* Beep command no longer exists

## Known bugs

Initial documentation for these versions have mentioned the following known bugs in the "`troubleshooting`" document: _(original wording preserved for brevity - only covers pre-2021 releases)_

<table><thead><tr><th width="101">Found</th><th width="94">Fixed</th><th>Description</th></tr></thead><tbody><tr><td>0.0.5.6</td><td>0.0.5.7</td><td><ol><li>[Unix] Any error messages that is handled will crash the handler with a continuable kernel error. Investigation found that something ambiguous is going on.</li></ol></td></tr></tbody></table>
