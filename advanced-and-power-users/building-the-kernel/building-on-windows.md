---
description: Build the simulator on Windows!
icon: windows
---

# Building on Windows

In Windows systems, you have two ways to build the simulator: one if you use Visual Studio 2022 or later, and one if you prefer doing everything via the command-line. Make sure that your computer has the following dependencies:

* [Git for Windows](https://git-scm.com/download/win)
* [.NET 8.0 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) (usually installed with Visual Studio 17.8 or later)

### Visual Studio 2022+

Before being able to build Nitrocid KS, please make sure that you have at least Visual Studio 2022 version 17.8 or later that supports building projects for .NET 8.0. You can get Visual Studio [here](https://visualstudio.microsoft.com/).

Once you have Visual Studio installed with at least the .NET 8.0 SDK and the .NET development workload, follow these steps:

1. Open Visual Studio and press `Clone Repository`
2.  In the repository location field, write `https://github.com/Aptivi/NitrocidKS.git`

    <div align="left">

    <figure><img src="../../.gitbook/assets/072-vsbuild.png" alt=""><figcaption></figcaption></figure>

    </div>
3. Press `Clone`. The clone may need to take a few minutes depending on your Internet connection.
4. Press `Solution Explorer` » `Switch Views` and double click on `Nitrocid.sln`\
   ![](../../.gitbook/assets/073-vsbuild.png)
5. Press `Start` or press `Build` » `Build Solution` to build\
   <img src="../../.gitbook/assets/074-vsbuild.png" alt="" data-size="original">
6.  Navigate to the build output folder, `KSBuild`, and double click on the `Nitrocid.exe` file\


    <figure><img src="../../.gitbook/assets/075-vsbuild.png" alt=""><figcaption></figcaption></figure>

### Using the command-line

If you are a hardcore command-line user or if you prefer using the command-line, follow these steps to build Nitrocid KS right from the command line:

1. Open `Git Bash` on your work directory
2. Execute `git clone https://github.com/Aptivi/NitrocidKS.git`
3. Navigate to the cloned repository, `NitrocidKS`
4. Execute `dotnet restore` and `dotnet build`
5.  After building is done, run `dotnet run` on the `Nitrocid.csproj` file like so:\


    <figure><img src="../../.gitbook/assets/076-vsbuild.png" alt=""><figcaption></figcaption></figure>
