---
description: Build the simulator on macOS!
icon: apple
---

# Building on macOS

In macOS systems, you can comfortably build Nitrocid KS using the command line, since it's the most lightweight solution. However, you must have the prerequisites before being able to build KS.

* [.NET 8.0 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/8.0)
* [Git for macOS](https://git-scm.com/download/mac)

### Using the command-line

If you are a hardcore command-line user or if you prefer using the command-line, follow these steps to build Nitrocid KS right from the command line:

1. Open your terminal emulator on your work directory
2. Execute `git clone https://github.com/Aptivi/NitrocidKS.git`
3. Navigate to the cloned repository, `NitrocidKS`
4. Execute `dotnet restore` and `dotnet build`
5. After building is done, run `dotnet run public/Nitrocid/Nitrocid.csproj`
