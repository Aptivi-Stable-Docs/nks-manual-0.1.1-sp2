---
description: Sometimes we need to check your platform.
icon: computer
---

# Kernel Platform

<figure><img src="../../../.gitbook/assets/134-inner.png" alt=""><figcaption></figcaption></figure>

Sometimes, your mod may contain code that only works on certain platforms. For example, your mod may intentionally call an external unmanaged library's function to do something not normally done in the .NET world using P/Invokes. However, native libraries need to be compiled for the target machines.

Or, you may want to change the behavior of your mod by platform or by how the kernel is started, such as if the kernel was started by the GRILO bootloader.

`KernelPlatform` provides functions to detect platforms and environments. This is to make such tasks relatively easy.

{% hint style="warning" %}
Currently, `KernelPlatform` doesn't support checking for architectures, like `IsAmd64()`.
{% endhint %}

You can check your kernel version and your host RID by going to `settings` > `System information`.

<figure><img src="../../../.gitbook/assets/133-inner.png" alt=""><figcaption></figcaption></figure>

## Operating systems

This class provides the following OS-related functions:

* `IsOnWindows()`: Checks to see if Nitrocid KS is running on Windows.
* `IsOnUnix()`: Checks to see if Nitrocid KS is running on Unix and its derivatives, such as Linux, macOS, etc.
* `IsOnMacOS()`: Checks to see if Nitrocid KS is running on Darwin Kernel, the kernel behind macOS.
* `IsOnAndroid()`: Checks to see if Nitrocid KS is running on Android phones and tablets within Termux.
* `IsOnUnixMusl()`: Checks to see if the kernel is running on musl libc library or glibc.

## Terminals

This class provides the following terminal-related functions:

* `GetTerminalEmulator()`: Gets the terminal emulator name. This may not be accurate.
* `GetTerminalType()`: Gets the terminal type name, such as xterm. This may not be accurate.

{% hint style="warning" %}
There are some terminal emulators advertising themselves as XTerm compliant, but actually aren't fully compliant. We advice that you introduce extra checks to ensure that your terminal emulator fully supports the features that you want to use, especially when it comes with the VT sequences.
{% endhint %}

## Environments

This class provides the following environment-related functions:

* `IsRunningFromGrilo()`: Checks to see whether Nitrocid KS is run from GRILO
* `IsRunningFromTmux()`: Checks to see whether Nitrocid KS is run on TMUX
* `IsRunningFromScreen()`: Checks to see whether Nitrocid KS is run on GNU Screen

## CoreCLR

This class provides the following CoreCLR-related functions:

* `GetCurrentRid()`: Gets the current runtime identifier.
* `GetCurrentGenericRid()`: Gets the current non-distro-specific runtime identifier.

{% hint style="info" %}
If you're running on a MUSL-based Linux system, `GetCurrentGenericRid()` returns a MUSL-based generic runtime identifier, such as `linux-musl-arm64`.
{% endhint %}
