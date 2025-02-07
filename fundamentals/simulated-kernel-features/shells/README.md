---
description: What are the shells?
icon: square-terminal
---

# Shells

<figure><img src="../../../.gitbook/assets/002-shell.png" alt=""><figcaption></figcaption></figure>

Operating systems contain their shells that listen to user input and execute any command – either built-in or custom – to perform operations that the users plan or need to do. Shells usually contain the prompt indicator to tell the user where is the current working directory, the user name, the host name, or anything that is set up to show.

In Linux systems, the shell is initiated with the help of both the log-in handler and the system initialization program, usually found in `/sbin/init`. Linux shells are numerous, like `zsh`, `dash`, and `bash`. In Windows, it has only two shells, which is PowerShell and Command Prompt (simulates the MS-DOS command prompt).

Each shell contain their own built-in commands and their usage and purpose that are hard-coded to the shell, and they're usually not distributed in a binary form. External commands are just programs or scripts that you would execute, but found in your system drive or any of the drives.

In the simulated kernel, the kernel initialization system only runs one shell, which is Unified Extensible SHell (UESH), that runs more like a shell manager. This shell manager calls for the main shell to be executed the moment the user logs on.

## Unified Extensible SHell (UESH)

This program is part of the core kernel that is programmed to not only be a standalone shell, but also be an extensible shell manager that actually executes built-in and custom shells, supervise the shell's behavior, and manage the shell stack. It provides you an ability to be able to implement your commands for any built-in shells and build your own shell that actually contains your own built-in commands.

For more information about its inner workings and how you can build your own commands and shells, consult the below page.

{% content-ref url="../../../advanced-and-power-users/inner-workings/shell-structure/" %}
[shell-structure](../../../advanced-and-power-users/inner-workings/shell-structure/)
{% endcontent-ref %}

For scripting, consult the below page.

{% content-ref url="../../../advanced-and-power-users/inner-workings/shell-structure/shell-scripting.md" %}
[shell-scripting.md](../../../advanced-and-power-users/inner-workings/shell-structure/shell-scripting.md)
{% endcontent-ref %}

## Commands

All of the shells contain their own commands that you can review on either the help command list (depending on the mode used, such as -addon for addon commands) or the below page (just for Nitrocid shells, base shell, and Addon commands):

{% content-ref url="commands-list.md" %}
[commands-list.md](commands-list.md)
{% endcontent-ref %}

{% content-ref url="addon-commands-list.md" %}
[addon-commands-list.md](addon-commands-list.md)
{% endcontent-ref %}

{% hint style="info" %}
You can also use `findcmds` to quickly find commands by regular expressions as demonstrated below.

<img src="../../../.gitbook/assets/062-findcmds.png" alt="" data-size="original">
{% endhint %}

### Preface for the `-set` switch

Some of the commands support the `-set` switch. This means that such commands can set a UESH variable to any value that the command sets without any interference.

Your custom mods can also use this feature to set the variable. To learn more, please visit the below link:

{% content-ref url="../../../advanced-and-power-users/inner-workings/shell-structure/" %}
[shell-structure](../../../advanced-and-power-users/inner-workings/shell-structure/)
{% endcontent-ref %}
