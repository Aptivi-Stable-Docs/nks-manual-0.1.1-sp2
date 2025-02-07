---
description: List of available commands
icon: square-code
---

# Commands List

This page is a reference that serves as a list of available commands. For addon commands, consult the below page here:

{% content-ref url="addon-commands-list.md" %}
[addon-commands-list.md](addon-commands-list.md)
{% endcontent-ref %}

## Built-in commands

Nitrocid KS currently provides the following commands (you can see their definitions in the help command list):

| Commands                | Arguments and Switches                                                           |
| ----------------------- | -------------------------------------------------------------------------------- |
| `addgroup`              | `<groupname>`                                                                    |
| `adduser`               | `<username> [password]`                                                          |
| `addusertogroup`        | `<username> <group>`                                                             |
| `admin`                 |                                                                                  |
| `alarm`                 | `<start/stop> <alarmname> [interval]`                                            |
|                         | `<list>`                                                                         |
|                         | `[-tui]`                                                                         |
| `alias`                 | `<rem/add> <shelltype> <alias> [cmd]`                                            |
| `beep`                  | `[freq] [ms]`                                                                    |
| `blockdbgdev`           | `<ipaddress>`                                                                    |
| `bulkrename`            | `<targetdir> <pattern> [newname]`                                                |
| `cat`                   | `[-lines\|-nolines\|-plain] <file>`                                              |
| `cdir`                  |                                                                                  |
| `changes`               |                                                                                  |
| `chattr`                | `<file> <add/rem> <attr>`                                                        |
| `chdir`                 | `<directory/..>`                                                                 |
| `chhostname`            | `<hostname>`                                                                     |
| `chklock`               | `[-waitforunlock] <file>`                                                        |
| `chlang`                | `[-usesyslang\|-user] <language>`                                                |
| `chmal`                 | `[message]`                                                                      |
| `chmotd`                | `[message]`                                                                      |
| `choice`                | `[-o\|-t\|-m\|-a] [-single\|-multiple] <answers> <input> [title] [title2] [...]` |
| `chpwd`                 | `<username> <pass> <newpass> <newpass>`                                          |
| `chusrname`             | `<oldusername> <newusername>`                                                    |
| `cls`                   |                                                                                  |
| `combinestr`            | `<input> <input2> [input3] [...]`                                                |
| `combine`               | `<output> <input> <input2> [input3] [...]`                                       |
| `convertlineendings`    | `[-w\|-u\|-m] [-force] <text>`                                                   |
| `copy`                  | `<source> <target>`                                                              |
| `date`                  | `[-date\|-time\|-full] [-utc]`                                                   |
| `debugshell`            |                                                                                  |
| `decodebase64`          | `<encoded>`                                                                      |
| `decodefile`            | `[-key] [-iv] <file> [algorithm]`                                                |
| `decodetext`            | `[-key] [-iv] <text> [algorithm]`                                                |
| `dirinfo`               | `<directory>`                                                                    |
| `disconndbgdev`         | `<ip>`                                                                           |
| `diskinfo`              | `<disknum>`                                                                      |
| `dismissnotif`          | `<num>`                                                                          |
| `dismissnotifs`         |                                                                                  |
| `driverman`             | `<list> <type>`                                                                  |
|                         | `<types>`                                                                        |
|                         | `<change> <type> <driverName>`                                                   |
| `echo`                  | `[-noparse] <text>`                                                              |
| `edit`                  | `[-text\|-sql\|-json\|-hex] <file>`                                              |
| `encodebase64`          | `<text>`                                                                         |
| `encodefile`            | `[-key] [-iv] <file> [algorithm]`                                                |
| `encodetext`            | `[-key] [-iv] <text> [algorithm]`                                                |
| `fileinfo`              | `<file>`                                                                         |
| `find`                  | `[-recursive] [-exec] <file> <directory>`                                        |
| `findcmds`              | `<search>`                                                                       |
| `findreg`               | `[-recursive] [-exec] <fileRegex> <directory>`                                   |
| `fork`                  |                                                                                  |
| `get`                   | `[-outputpath] <url>`                                                            |
| `getaddons`             | `[-reinstall]`                                                                   |
| `getallexthandlers`     |                                                                                  |
| `getconfigvalue`        | `[-set=variable] <config> <variable>`                                            |
| `getdefaultexthandler`  | `<extension>`                                                                    |
| `getdefaultexthandlers` |                                                                                  |
| `getexthandlers`        | `<extension>`                                                                    |
| `getkeyiv`              | `[algorithm]`                                                                    |
| `host`                  |                                                                                  |
| `hwinfo`                | `<type>`                                                                         |
| `if`                    | `<expression> <command>`                                                         |
| `ifm`                   |                                                                                  |
| `input`                 | `<question>`                                                                     |
| `inputpass`             | `<question>`                                                                     |
| `langman`               | `<reload/load/unload> <customlangname>`                                          |
|                         | `<list/reloadall>`                                                               |
| `license`               |                                                                                  |
| `lintscript`            | `<script>`                                                                       |
| `list`                  | `[-showdetails] [-suppressmessages] [-recursive\|-tree] [directory]`             |
| `lockscreen`            |                                                                                  |
| `logout`                |                                                                                  |
| `lsconfigs`             | `-deep`                                                                          |
| `lsconfigvalues`        | `<config>`                                                                       |
| `lsconnections`         |                                                                                  |
| `lsdbgdev`              |                                                                                  |
| `lsdiskparts`           | `<disknum>`                                                                      |
| `lsdisks`               |                                                                                  |
| `lsexthandlers`         |                                                                                  |
| `lsnet`                 |                                                                                  |
| `lsvars`                |                                                                                  |
| `md`                    | `<directory>`                                                                    |
| `mkfile`                | `<file>`                                                                         |
| `modman`                | `<start/stop/info/reload/install/uninstall> <modfilename>`                       |
|                         | `<list/reloadall/stopall/startall>`                                              |
| `modmanual`             | `<modname>`                                                                      |
| `move`                  | `<source> <target>`                                                              |
| `partinfo`              | `<disknum> <partnum>`                                                            |
| `pathfind`              | `<filename>`                                                                     |
| `perm`                  | `<username> <allow/revoke> <perm>`                                               |
| `permgroup`             | `<groupname> <allow/revoke> <perm>`                                              |
| `ping`                  | `[-times] <address1> [address2] [...]`                                           |
| `platform`              | `[-r\|-v\|-b\|-c\|-n]`                                                           |
| `put`                   | `<filename> <url>`                                                               |
| `rdebug`                |                                                                                  |
| `reboot`                | `[-safe\|-maintenance\|-debug]`                                                  |
| `reloadconfig`          |                                                                                  |
| `retroks`               |                                                                                  |
| `rexec`                 | `<address> <port> <command>`                                                     |
| `rm`                    | `<target>`                                                                       |
| `rmsec`                 | `<target>`                                                                       |
| `rmuser`                | `<username>`                                                                     |
| `rmgroup`               | `<groupname>`                                                                    |
| `rmuserfromgroup`       | `<username> <groupname>`                                                         |
| `rreboot`               | `[-safe\|-maintenance\|-debug] [ip] [port]`                                      |
| `rshutdown`             | `[ip] [port]`                                                                    |
| `saveconfig`            |                                                                                  |
| `savescreen`            | `[-select] [saver]`                                                              |
| `search`                | `<regex> <file>`                                                                 |
| `searchword`            | `<phrase> <file>`                                                                |
| `select`                | `<answers> <input> [title] [title2] [...]`                                       |
| `setexthandler`         | `<extension> <implementer>`                                                      |
| `setsaver`              | `<saver>`                                                                        |
| `settings`              | `[-saver\|-addonsaver\|-splash\|-driver\|-type=typename] [-sel]`                 |
| `set`                   | `<value>`                                                                        |
| `setrange`              | `<value> [value2] [value3] [...]`                                                |
| `shownotifs`            | `[-tui]`                                                                         |
| `showtd`                |                                                                                  |
| `showtdzone`            | `[-all] [-selection] <timezone>`                                                 |
| `shutdown`              |                                                                                  |
| `sleep`                 | `<ms>`                                                                           |
| `sudo`                  | `<command>`                                                                      |
| `sumfile`               | `[-relative] <algorithm/all> <file> [output]`                                    |
| `sumfiles`              | `[-relative] <algorithm/all> <dir> [output]`                                     |
| `sumtext`               | `<algorithm/all> <text>`                                                         |
| `symlink`               | `<linkname> <target>`                                                            |
| `sysinfo`               | `[-s\|-h\|-u\|-m\|-l\|-a]`                                                       |
| `taskman`               |                                                                                  |
| `themeprev`             | `[theme]`                                                                        |
| `themeset`              | `[-y] [theme]`                                                                   |
| `unblockdbgdev`         | `<address>`                                                                      |
| `unset`                 | `[-justwipe] <$variable>`                                                        |
| `unzip`                 | `[-createdir] <zipfile> [path]`                                                  |
| `update`                |                                                                                  |
| `uptime`                |                                                                                  |
| `usermanual`            |                                                                                  |
| `verify`                | `<algorithm> <calculatedhash> <hashfile/expectedhash> <file>`                    |
| `version`               | `[-m\|-k]`                                                                       |
| `whoami`                |                                                                                  |
| `winelevate`            |                                                                                  |
| `wraptext`              | `[-columns=num] <file>`                                                          |
| `zip`                   | `[-fast\|-nocomp] [-nobasedir] <zipfile> <path>`                                 |

## Unified commands

The below commands are available to all the shells, either built-in or your custom shells:

| Commands        | Arguments and Switches                                       |
| --------------- | ------------------------------------------------------------ |
| `exec`          | `[-forked] <process> [args]`                                 |
| `exit`          |                                                              |
| `help`          | `[-general\|-mod\|-alias\|-addon\|-unified\|-all] [command]` |
| `loadhistories` |                                                              |
| `presets`       |                                                              |
| `repeat`        | `<times> [command]`                                          |
| `savehistories` |                                                              |
| `tip`           |                                                              |
| `wrap`          | `<command>`                                                  |

## Other shells

In addition to the built-in commands, we also have commands for other shells.

### Admin shell

This shell provides you administrative tools. The following commands are available:

| Commands           | Arguments and Switches                           |
| ------------------ | ------------------------------------------------ |
| `arghelp`          | `[argument]`                                     |
| `bootlog`          |                                                  |
| `cdbglog`          |                                                  |
| `clearfiredevents` |                                                  |
| `journal`          |                                                  |
| `lsevents`         |                                                  |
| `lsusers`          |                                                  |
| `userflag`         | `<user> <admin/anonymous/disabled> <false/true>` |
| `userfullname`     | `<user> <name/clear>`                            |
| `userinfo`         | `[user]`                                         |
| `userlang`         | `<user> <lang/clear>`                            |

### Debug shell

This shell provides debug information. Here are the currently supported commands:

| Commands           | Arguments and Switches                 |
| ------------------ | -------------------------------------- |
| `currentbt`        |                                        |
| `debuglog`         | `<sessionnum>`                         |
| `excinfo`          | `<excnum>`                             |
| `getfieldvalue`    | `<field>`                              |
| `getpropertyvalue` | `<property>`                           |
| `keyinfo`          |                                        |
| `lsaddonfields`    | `<addon>`                              |
| `lsaddonfuncs`     | `<addon>`                              |
| `lsaddonprops`     | `<addon>`                              |
| `lsaddons`         |                                        |
| `lsbaseaddons`     |                                        |
| `lsfields`         | `[-suppress]`                          |
| `lsproperties`     | `[-suppress]`                          |
| `lsshells`         |                                        |
| `previewsplash`    | `[-splashout] [-context] <splashname>` |
| `showmainbuffer`   |                                        |

### Hex Shell

The hex shell allows you to edit files byte by byte. You can use the following commands:

| Commands     | Arguments and Switches         |
| ------------ | ------------------------------ |
| `addbyte`    | `<byte>`                       |
| `addbytes`   |                                |
| `addbyteto`  | `<byte> <pos>`                 |
| `clear`      |                                |
| `delbyte`    | `<bytenumber>`                 |
| `delbytes`   | `<startbyte> [endbyte]`        |
| `exitnosave` |                                |
| `print`      | `[startbyte] [endbyte]`        |
| `querybyte`  | `<byte> [startbyte] [endbyte]` |
| `replace`    | `<byte> <replacedbyte>`        |
| `save`       |                                |
| `tui`        |                                |

### Text Shell

The text editor shell allows you to easily manipulate with the text files. You can use the below commands:

| Commands             | Arguments and Switches                                 |
| -------------------- | ------------------------------------------------------ |
| `addline`            | `<text>`                                               |
| `addlines`           |                                                        |
| `clear`              |                                                        |
| `delcharnum`         | `<charnum> <linenum>`                                  |
| `delline`            | `<linenum> [linenum2]`                                 |
| `delword`            | `<word/phrase> <linenum> [linenum2]`                   |
| `editline`           | `<linenum>`                                            |
| `exitnosave`         |                                                        |
| `print`              | `[linenum] [linenum2]`                                 |
| `querychar`          | `<char> <linenum/all> [linenum2]`                      |
| `queryword`          | `<word/phrase> <linenum/all> [linenum2]`               |
| `querywordregex`     | `<regex> <linenum/all> [linenum2]`                     |
| `replace`            | `<word/phrase> <word/phrase>`                          |
| `replaceinline`      | `<word/phrase> <word/phrase> <linenum/all> [linenum2]` |
| `replaceregex`       | `<regex> <word/phrase>`                                |
| `replaceinlineregex` | `<regex> <word/phrase> <linenum/all> [linenum2]`       |
| `save`               |                                                        |
