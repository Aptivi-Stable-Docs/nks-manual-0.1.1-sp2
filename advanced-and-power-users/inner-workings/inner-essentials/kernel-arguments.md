---
description: How do the kernel arguments work? And how it affects the kernel?
icon: square-sliders
---

# Kernel Arguments

<figure><img src="../../../.gitbook/assets/129-inner.png" alt=""><figcaption></figcaption></figure>

Kernel Arguments are command-line parameters to the simulator that changes the behavior of the kernel according to the available arguments listed below:

* `attach`: Attaches a Visual Studio debugger to the current instance of the kernel (Windows only)
* `quiet`: Starts the kernel quietly
* `maintenance`: Starts the kernel in maintenance mode which behaves like safe mode but with additional features turned off
* `safe`: Starts the kernel in safe mode which disables all mods
* `testInteractive`: Opens the interactive test facade selection
* `debug`: Enables debug mode
* `terminaldebug`: Enables terminal debug mode
* `reset`: Wipes all settings and resets the kernel to factory settings
* `bypasssizedetection`: Bypasses the 80x24 console size detection
* `noaltbuffer`: Prevents the kernel from using the alternative buffer
* `noprebootsplash`: Prevents the kernel from displaying the pre-boot splash
* `lang <lang>`: Selects a pre-boot environment language
* `verbosepreboot`: Shows extra pre-boot messages

Each argument is provided the `ArgumentInfo` class that defines the argument and its properties.

{% hint style="warning" %}
It's been theorized that we can actually define the argument parameters using `CommandArgumentInfo`. As of Beta 1, there is one kernel argument that actually uses parameters, but you have to surround said argument with the double quotation marks `""`.
{% endhint %}

The arguments are parsed each time the kernel starts up or gets rebooted. If there are any switches or argument parameters, they'll get parsed using the `ProvidedArgumentsInfo` class.

{% hint style="info" %}
To learn more about `ProvidedArgumentsInfo`, click on the below link:

[command-parsing.md](../shell-structure/command-parsing.md "mention")
{% endhint %}
