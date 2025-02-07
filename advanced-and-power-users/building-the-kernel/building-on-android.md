---
description: Build the simulator on Android!
icon: android
---

# Building on Android

In Android phones and tablets with Termux installed with proot, you can comfortably build Nitrocid KS using the command line, since it's the most lightweight solution. However, you must have the prerequisites before being able to build KS.

* [.NET 8.0 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/8.0)
* [Termux](https://f-droid.org/en/packages/com.termux/)

{% hint style="info" %}
Ubuntu 23.10 Mantic Minotaur users or later can directly download .NET 8.0 SDK from the official Ubuntu repositories without having to import the Microsoft's repositories.

You may need to manually update your Ubuntu proot distribution to either Mantic Minotaur or to Noble Numbat to be able to use .NET 8.0 SDK.
{% endhint %}

### Using the command-line

If you are a hardcore command-line user or if you prefer using the command-line, follow these steps to build Nitrocid KS right from the command line:

1. Open your terminal emulator on your work directory
2. Execute `git clone https://github.com/Aptivi/NitrocidKS.git`
3. Navigate to the cloned repository, `NitrocidKS`
4. Execute `dotnet restore` and `dotnet build`
5. After building is done, run `dotnet run public/Nitrocid/Nitrocid.csproj`
