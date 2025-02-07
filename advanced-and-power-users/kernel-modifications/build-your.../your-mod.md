---
description: >-
  This page describes how to make your own custom modification using Visual
  Studio.
---

# 🧪 Building your Mod

You're looking to create a mod for Nitrocid KS! That's great! Make sure that you have Visual Studio installed. Follow the steps to create your first mod.

1.  Press `Create a new project` on Visual Studio homepage

    <figure><img src="../../../.gitbook/assets/077-modbuild.png" alt=""><figcaption></figcaption></figure>
2.  Find `Class Library` and double-click it

    <figure><img src="../../../.gitbook/assets/078-modbuild.png" alt=""><figcaption></figcaption></figure>
3.  Write your mod name, like in our example, `MyFirstMod`.

    <figure><img src="../../../.gitbook/assets/079-modbuild.png" alt=""><figcaption></figcaption></figure>
4.  Make sure that `.NET 8.0` is used. Press `Create`.

    <figure><img src="../../../.gitbook/assets/080-modbuild.png" alt=""><figcaption></figcaption></figure>
5.  Right-click on `Dependencies` -> `Manage NuGet Packages`, find `KS`, and install it.

    <figure><img src="../../../.gitbook/assets/081-modbuild.png" alt=""><figcaption></figcaption></figure>
6.  Once the package is installed, go to the `Class1.cs` source file

    <figure><img src="../../../.gitbook/assets/082-modbuild.png" alt=""><figcaption></figcaption></figure>
7.  Write next to the class file `: IMod` and import the required namespace `by using Nitrocid.Modifications;`. You should see errors indicating that you have to implement the methods.

    <figure><img src="../../../.gitbook/assets/083-modbuild.png" alt=""><figcaption></figcaption></figure>
8.  After implementation, you should see:

    <figure><img src="../../../.gitbook/assets/084-modbuild.png" alt=""><figcaption></figcaption></figure>
9.  Remove all calls to `NotImplementedException` and set the minimum supported API version. Currently, we're at `v3.0.25.437`.

    <figure><img src="../../../.gitbook/assets/085-modbuild.png" alt=""><figcaption></figcaption></figure>
10. Now, implement everything as you wish. Once you're done, follow the steps on how to create a strong name signing key and [sign your mod](https://learn.microsoft.com/en-us/dotnet/standard/assembly/sign-strong-name). Then, click on the `Build` menu and select `Build Solution`.

    <div align="left">

    <figure><img src="../../../.gitbook/assets/086-modbuild.png" alt=""><figcaption></figcaption></figure>

    </div>
11. Once the solution is built, open the file explorer to the solution directory by right-clicking on the solution and selecting `Open Folder in File Explorer`.

    <div align="left">

    <figure><img src="../../../.gitbook/assets/087-modbuild.png" alt=""><figcaption></figcaption></figure>

    </div>
12. Navigate to the output directory and copy the `.dll` file to `KSMods` under the `%localappdata%/KS` folder.

    <figure><img src="../../../.gitbook/assets/088-modbuild.png" alt=""><figcaption></figcaption></figure>
13. Open Nitrocid KS to test your mod. Ensure that `modman list` lists your mod.

    <figure><img src="../../../.gitbook/assets/089-modbuild.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
You will have to turn on kernel modifications from the kernel settings. Navigate to `General > Start kernel modifications on boot` and turn it on.

You can also make use of the [`KSTemplates`](https://github.com/Aptivi/KSTemplates) repository, which can be installed to Visual Studio using the `dotnet new install path/to/KS.Templates.nupkg` command.
{% endhint %}
