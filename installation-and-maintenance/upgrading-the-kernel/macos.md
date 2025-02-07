---
description: How to upgrade Nitrocid KS on macOS
icon: apple
---

# macOS

{% hint style="warning" %}
The upgrade instructions is based on pre-release version of Nitrocid KS, and the system requirements may change during the development.
{% endhint %}

The only way to upgrade your kernel in macOS is to unpack the updated kernel files manually. This method also works for bleeding-edge builds, though you have to use unzip instead. To upgrade, follow these steps:

1. Download the latest release ZIP file from [this page](https://github.com/Aptivi/Kernel-Simulator/releases).
2. Unpack the ZIP archive to any folder of your choice
3. Open your favorite terminal emulator, like iTerm2, and change the working directory to a folder containing the Nitrocid KS executable
4. Execute `dotnet Nitrocid.dll` to start the kernel

{% hint style="info" %}
For 0.0.24.x or older, files that end with the `-dotnet` prefix means that it's for .NET 6.0.
{% endhint %}
