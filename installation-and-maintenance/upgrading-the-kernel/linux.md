---
description: How to upgrade Nitrocid KS on Linux
icon: linux
---

# Linux

Upgrading the kernel on Linux is simple, depending on the way you've installed the kernel. To upgrade the kernel, choose your preferred method.

### Method 1: Manually unpacking

If you'd like to manually update your kernel to the latest version or to the latest bleeding-edge version, follow these steps:

1. Ensure that you have all the required software installed
2. Download the latest release ZIP file from [this page](https://github.com/Aptivi/Kernel-Simulator/releases).
3. Unpack the ZIP archive to any folder of your choice
4. Open your favorite terminal emulator, like Konsole, and change the working directory to a folder containing the Nitrocid KS executable
5. Execute `dotnet Nitrocid.dll` to start the kernel

{% hint style="info" %}
For 0.0.24.x or older, files that end with the `-dotnet` prefix means that it's for .NET 6.0.
{% endhint %}

### Method 2: Ubuntu PPA

If you're running Ubuntu, you can update KS using the Ubuntu PPA. Just follow these steps:

1. Open your terminal emulator and execute the following command, assuming that you've already imported the Ubuntu PPA.
   * `sudo apt update`
2. Install the `kernel-simulator` package
   * `sudo apt install kernel-simulator`
3. Start `ks` or use your app drawer to find `Nitrocid KS`

Once you upgrade KS using this method, you can't use your package manager to downgrade the version, so update accordingly. If you really want to downgrade the package, the only safe way to downgrade is to unpack the kernel manually.
