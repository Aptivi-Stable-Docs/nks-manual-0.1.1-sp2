---
description: List of available addon commands
icon: square-code
---

# Addon Commands List

This page is a reference that serves as a list of available addon commands.

## Addon commands

Additionally, addons provide commands that you can use to get the most out of them. They are available as part of the Extras addon.

### Amusements

Amusements addon provides you with a pack of games and amusements. You can use the below commands:

| Commands     | Arguments and Switches                        |
| ------------ | --------------------------------------------- |
| `backrace`   |                                               |
| `hangman`    | `[-uncommon\|-common] [-hardcore\|-practice]` |
| `meteor`     |                                               |
| `pong`       |                                               |
| `quote`      |                                               |
| `roulette`   |                                               |
| `shipduet`   |                                               |
| `snaker`     |                                               |
| `solver`     |                                               |
| `speedpress` | `[-e\|-m\|-h\|-v\|-c]`                        |
| `wordle`     | `[-uncommon\|-common] [-orig]`                |
| `2018`       |                                               |

### Archive shell

This addon includes an archive shell which lets you manipulate with the archive files. On startup, the below commands are added to the UESH shell:

| Commands  | Arguments and Switches |
| --------- | ---------------------- |
| `archive` | `<archivefile>`        |

Once you enter the archive shell with the provided archive file path, you can access the below archive shell commands:

| Commands | Arguments and Switches        |
| -------- | ----------------------------- |
| `cdir`   |                               |
| `chdir`  | `<directory>`                 |
| `chadir` | `<archivedir>`                |
| `get`    | `[-absolute] <entry> [where]` |
| `list`   | `[directory]`                 |
| `pack`   | `<localfile> [where]`         |

### BassBoom Addon

This addon allows you to get access to the BassBoom utility features. You can use the following commands:

| Commands      | Arguments and Switches        |
| ------------- | ----------------------------- |
| `lyriclines`  | `<lyric.lrc>`                 |
| `playlyric`   | `<lyric.lrc>`                 |
| `playsound`   | `<musicfile>`                 |
| `musicplayer` | `-r [musicfile\|stationurl]`  |
| `netfminfo`   | `[-secure] <hostname> <port>` |

### Beep Synth Addon

This addon allows you to play beep synth files using the following commands:

| Commands    | Arguments and Switches |
| ----------- | ---------------------- |
| `beepsynth` | `<synthfile>`          |

### Caffeine Addon

This addon lets you be alarmed when your cup of tea or coffee is ready. You can use the following commands:

| Commands   | Arguments and Switches |
| ---------- | ---------------------- |
| `caffeine` | `[-abort] <seconds>`   |

### Calculators Addon

This addon lets you use different calculators. Use the following commands to get started:

| Commands    | Arguments and Switches |
| ----------- | ---------------------- |
| `calc`      | `<expression>`         |
| `imaginary` | `<real> <imaginary>`   |

### Calendar Addon

This addon allows you access to the calendar functions. You can use the below commands:

| Commands   | Arguments and Switches                            |
| ---------- | ------------------------------------------------- |
| `altdate`  | `[-date\|-time\|-full] [-utc] <culture>`          |
| `calendar` | `<tui> [-calendar=type] [-legacy] [year] [month]` |
|            | `<event> <add> <date> <title>`                    |
|            | `<event> <remove> <eventid>`                      |
|            | `<event> <list>`                                  |
|            | `<event> <saveall>`                               |
|            | `<reminder> <add> <dateandtime> <title>`          |
|            | `<reminder> <remove> <reminderid>`                |
|            | `<reminder> <list>`                               |
|            | `<reminder> <saveall>`                            |

### Color Conversion

This addon gives you various color model translation commands. You can use the following commands:

| Commands         | Arguments and Switches                                                        |
| ---------------- | ----------------------------------------------------------------------------- |
| `colorspecto`    | `<targetModelName> <specifier>`                                               |
| `colorspectohex` | `<specifier>`                                                                 |
| `colorspectoks`  | `<targetModelName> <specifier>`                                               |
| `colorto`        | `<sourceModelName> <targetModelName> <number1> <number2> <number3> [number4]` |
| `colortohex`     | `<sourceModelName> <number1> <number2> <number3> [number4]`                   |
| `colortoks`      | `<sourceModelName> <targetModelName> <number1> <number2> <number3> [number4]` |

### Contacts

You can use the following commands provided by the contacts addon:

| Commands         | Arguments and Switches    |
| ---------------- | ------------------------- |
| `contacts`       |                           |
| `listcontacts`   |                           |
| `loadcontacts`   |                           |
| `importcontacts` | `[-mecard] <mecard/path>` |
| `contactinfo`    | `<contactNum>`            |

### Diagnostics

You can use the following commands provided by the diagnostics addon:

| Commands    | Arguments and Switches |
| ----------- | ---------------------- |
| `threadsbt` |                        |

### Dictionary

You can use the following commands provided by the dictionary addon:

| Commands | Arguments and Switches |
| -------- | ---------------------- |
| `dict`   | `<word>`               |

### Docking

You can use the following commands provided by the screen docking addon:

| Commands | Arguments and Switches |
| -------- | ---------------------- |
| `dock`   | `<dockId>`             |

### Forecast

The forecast addon allows you to take a look at the current forecast across the world. You can use the following commands:

| Commands      | Arguments and Switches                                |
| ------------- | ----------------------------------------------------- |
| `weather`     | `[-tui] [-list=name] <latitude> <longitude> [apikey]` |
| `weather-old` | `[-list] <cityid/cityname> [apikey]`                  |

### FTP Shell

This shell provides you a client to the FTP servers. Here is a list of supported commands:

| Commands    | Arguments and Switches                   |
| ----------- | ---------------------------------------- |
| `cat`       | `<file>`                                 |
| `cdl`       | `<directory>`                            |
| `cdr`       | `<directory>`                            |
| `cp`        | `<source> <target>`                      |
| `del`       | `<file>`                                 |
| `detach`    |                                          |
| `execute`   | `<command>`                              |
| `get`       | `<file> [where]`                         |
| `getfolder` | `<folder> [where]`                       |
| `info`      |                                          |
| `lsl`       | `[-showdetails\|suppressmessages] [dir]` |
| `lsr`       | `[-showdetails] [dir]`                   |
| `mv`        | `<source> <target>`                      |
| `put`       | `<file> [output]`                        |
| `putfolder` | `<folder> [output]`                      |
| `pwdl`      |                                          |
| `pwdr`      |                                          |
| `perm`      | `<file> <permnum>`                       |
| `sumfile`   | `<file> <algorithm>`                     |
| `sumfiles`  | `<directory> <algorithm>`                |
| `type`      | `<a/b>`                                  |

### Git Shell

This addon includes a Git shell which lets you practice version controlling. On startup, the below commands are added to the UESH shell:

| Commands | Arguments and Switches |
| -------- | ---------------------- |
| `gitsh`  | `<repopath>`           |

Once you enter the Git shell with the provided repo path, you can access the below archive shell commands:

| Commands     | Arguments and Switches   |
| ------------ | ------------------------ |
| `blame`      |                          |
| `checkout`   | `<branch>`               |
| `commit`     | `<summary>`              |
| `describe`   | `<commitsha>`            |
| `diff`       | `[-tree\|-patch\|-all]`  |
| `fetch`      | `[remote]`               |
| `filestatus` | `<file>`                 |
| `info`       |                          |
| `lsbranches` |                          |
| `lscommits`  |                          |
| `lsremotes`  |                          |
| `lstags`     |                          |
| `maketag`    | `<tagname> [message]`    |
| `pull`       |                          |
| `push`       |                          |
| `reset`      | `[-soft\|-hard\|-mixed]` |
| `setid`      | `<email> <username>`     |
| `stage`      | `<unstagedfile>`         |
| `stageall`   |                          |
| `status`     |                          |
| `unstage`    | `<stagedfile>`           |
| `unstageall` |                          |

### HTTP Shell

The HTTP shell allows you to interact with an HTTP server, like sending requests to it. You can use the below commands:

| Commands     | Arguments and Switches   |
| ------------ | ------------------------ |
| `addheader`  | `<key> <value>`          |
| `curragent`  |                          |
| `delete`     | `<request>`              |
| `detach`     |                          |
| `editheader` | `<key> <value>`          |
| `get`        | `<request>`              |
| `getstring`  | `<request>`              |
| `lsheader`   |                          |
| `put`        | `<request> <pathtofile>` |
| `putstring`  | `<request> <string>`     |
| `post`       | `<request> <pathtofile>` |
| `poststring` | `<request> <string>`     |
| `rmheader`   | `<key>`                  |
| `setagent`   | `<useragent>`            |

### JSON Shell

The JSON shell allows you to easily edit JSON files. You can use the below commands:

| Commands       | Arguments and Switches                                            |
| -------------- | ----------------------------------------------------------------- |
| `add`          | `<-type=value> [-parentPath=value] [-propName=value] <jsonValue>` |
| `clear`        |                                                                   |
| `exitnosave`   |                                                                   |
| `findproperty` | `[-parentproperty] <propname>`                                    |
| `jsoninfo`     | `[-simplified] [-showvals]`                                       |
| `jsonminify`   | `<jsonfile> <output>`                                             |
| `jsonbeautify` | `<jsonfile> <output>`                                             |
| `jsondiff`     | `<file1> <file2>`                                                 |
| `print`        | `[propname]`                                                      |
| `rm`           | `<objectPath>`                                                    |
| `save`         | `[-b\|-m]`                                                        |
| `set`          | `<-type=value> [-parentPath=value] [-propName=value] <jsonValue>` |
| `tui`          |                                                                   |

### Language Studio

You can use the following commands to build your own language:

| Commands | Arguments and Switches        |
| -------- | ----------------------------- |
| `mklang` | `[-tui] <pathtotranslations>` |

### Name generator

You can generate names by using the following commands:

| Commands        | Arguments and Switches                                                        |
| --------------- | ----------------------------------------------------------------------------- |
| `findfirstname` | `[-t] [term] [nameprefix] [namesuffix]`                                       |
| `findsurname`   | `[term] [surnameprefix] [surnamesuffix]`                                      |
| `genname`       | `[-t] <namescount> [nameprefix] [namesuffix] [surnameprefix] [surnamesuffix]` |

### Mail Shell

This shell allows you to check your e-mails and send them. You can use the following commands:

| Commands  | Arguments and Switches            |
| --------- | --------------------------------- |
| `cd`      | `<folder>`                        |
| `detach`  |                                   |
| `lsdirs`  |                                   |
| `list`    | `[pagenum]`                       |
| `mkdir`   | `<foldername>`                    |
| `mv`      | `<mailid> <targetfolder>`         |
| `mvall`   | `<sendername> <targetfolder>`     |
| `read`    | `<mailid>`                        |
| `readenc` | `<mailid>`                        |
| `ren`     | `<oldfoldername> <newfoldername>` |
| `rm`      | `<mailid>`                        |
| `rmall`   | `<sendername>`                    |
| `rmdir`   | `<foldername>`                    |
| `send`    |                                   |
| `sendenc` |                                   |

### Pastebin

This shell allows you to send either a file or a string to one of the pastebin providers. You can use the following commands:

| Commands   | Arguments and Switches                                                                                               |
| ---------- | -------------------------------------------------------------------------------------------------------------------- |
| `pastebin` | `[-provider=address:host\|-type=raw/http/https\|-postpage=path/to/remote/post\|-postformat=text/json] <file/string>` |

### RSS Shell

You can use this shell to read your RSS feeds using the `rss` command. You can use the below commands:

| Commands       | Arguments and Switches       |
| -------------- | ---------------------------- |
| `articleinfo`  | `<feednum>`                  |
| `bookmark`     |                              |
| `detach`       |                              |
| `feedinfo`     |                              |
| `list`         |                              |
| `listbookmark` |                              |
| `read`         | `<feednum>`                  |
| `selfeed`      | `<phrase>`                   |
| `search`       | `[-t\|-d\|-a\|-cs] <phrase>` |
| `unbookmark`   |                              |

### SFTP Shell

SFTP shell allows you to interact with the SFTP servers.

| Commands | Arguments and Switches                    |
| -------- | ----------------------------------------- |
| `cat`    | `<file>`                                  |
| `cdl`    | `<dir>`                                   |
| `cdr`    | `<dir>`                                   |
| `del`    | `<file>`                                  |
| `detach` |                                           |
| `get`    | `<file>`                                  |
| `lsl`    | `[-showdetails\|-suppressmessages] [dir]` |
| `lsr`    | `[-showdetails] [dir]`                    |
| `put`    | `<file>`                                  |
| `pwdl`   |                                           |
| `pwdr`   |                                           |

### Notes

You can jot down notes by using the following commands:

| Commands      | Arguments and Switches |
| ------------- | ---------------------- |
| `addnote`     | `<contents>`           |
| `listnotes`   |                        |
| `notestui`    |                        |
| `reloadnotes` |                        |
| `removenote`  | `<notenum>`            |
| `removenotes` |                        |
| `savenotes`   |                        |

### SQL Shell

This shell allows you to execute commands from the connected SQLite database file. You can use the below commands:

| Commands | Arguments and Switches |
| -------- | ---------------------- |
| `cmd`    | `<query>`              |
| `dbinfo` |                        |
| `tui`    |                        |

### Stocks

This addon provides you with the following commands to query stock price info:

| Commands | Arguments and Switches |
| -------- | ---------------------- |
| `stock`  | `[company]`            |

### Theme Studio

You can make your own precious and epic themes by using the following commands:

| Commands  | Arguments and Switches |
| --------- | ---------------------- |
| `mktheme` | `[-tui] <themename>`   |

### Time info

You can get detailed time info by using the following commands:

| Commands      | Arguments and Switches |
| ------------- | ---------------------- |
| `gettimeinfo` | `[-now] <date>`        |

### Timers

You can use the timer and the stopwatch using the following commands:

| Commands    | Arguments and Switches |
| ----------- | ---------------------- |
| `stopwatch` |                        |
| `timer`     |                        |

### To-do List

You can build your own task list using the following commands:

| Commands | Arguments and Switches |
| -------- | ---------------------- |
| `todo`   | `<add> <taskname>`     |
|          | `<remove> <taskname>`  |
|          | `<done> <taskname>`    |
|          | `<undone> <taskname>`  |
|          | `<list>`               |
|          | `<save>`               |
|          | `<load>`               |

### Unit conversion

You can initiate unit conversion using the following commands:

| Commands    | Arguments and Switches                                   |
| ----------- | -------------------------------------------------------- |
| `listunits` | `<type>`                                                 |
| `unitconv`  | `[-tui] <unittype> <quantity> <sourceunit> <targetunit>` |
