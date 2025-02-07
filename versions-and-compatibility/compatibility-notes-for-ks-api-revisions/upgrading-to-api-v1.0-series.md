---
description: Follow the compatibility notes when upgrading your mods to API v1.0 series
---

# 🔼 Upgrading to API v1.0 series

When upgrading your modification from the target of the later version of Nitrocid KS that declares itself to be from the API v1.0, you must make necessary changes to be able to use your mod in a Nitrocid KS version which you use to test your mod. These changes will be listed starting from 0.0.4 to the last version in this API revision.

## From 0.0.1 to 0.0.4

Although this version is the first version which supported the kernel modification feature, these below functions are removed for reasons stated below each function.

{% content-ref url="../version-release-notes/v0.0.4.x-series.md" %}
[v0.0.4.x-series.md](../version-release-notes/v0.0.4.x-series.md)
{% endcontent-ref %}

### `ResetTimeDate()`

{% code title="TimeDate.vb" lineNumbers="true" %}
```visual-basic
Sub ResetTimeDate()
```
{% endcode %}

This function first appeared in 0.0.3 as part of the changes that happened between 0.0.2 and the aforementioned version. However, this function only nullified the values of the kernel time and date variables, making this function rather redundant. It was removed.

{% hint style="danger" %}
We advice you to cease using this function.
{% endhint %}

### `permissionEdit()`

{% code title="Groups.vb" lineNumbers="true" %}
```visual-basic
Sub permissionEdit(ByVal username As String, ByVal type As String, ByVal allowed As Boolean)
```
{% endcode %}

This function first appeared with the initial debut of the group system. However, it got removed because, according to the analysis made in 2021, `permission()` function already did its job. Further examination revealed that it was only trying to edit the permission for a user, but, in the process of doing so, it cloned what `permission()` would do.

{% hint style="info" %}
The new permission system is here, and you can make use of it to grant and deny users a specific permission. Their method signatures are written below:

```csharp
public static void GrantPermission(string User, PermissionTypes permissionType)
public static void RevokePermission(string User, PermissionTypes permissionType)
```
{% endhint %}

### `ProbeBIOS()`

{% code title="HardwareProbe.vb" lineNumbers="true" %}
```visual-basic
Sub ProbeBIOS(Optional ByVal QuietBIOS As Boolean = False)
```
{% endcode %}

This function was added to "simplify" silencing the messages being seen when the BIOS probing started. The boolean value in the function indicated if the messages are going to be suppressed during the BIOS probing. However, it was removed as unnecessary because it was a wrapper to the function that probed the computer about the information of the BIOS.

{% hint style="danger" %}
We advice you to cease using this function.
{% endhint %}

## From 0.0.4 to 0.0.4.5

This version incorporated a few changes to the kernel, including the below API changes that may impact your mods.

### `ShowTimeQuiet()`

{% code title="TimeDate.vb" lineNumbers="true" %}
```visual-basic
Sub ShowTimeQuiet()
```
{% endcode %}

This function was created back in 0.0.2 to wrap the conditional execution of `ShowTime()`. This was generally a shortcut to checking to see if the kernel quiet mode is enabled. However, it was deemed unnecessary, and, thus, removed.

{% hint style="danger" %}
Latest generations of Nitrocid KS offer better quiet mode, which functions more dynamically than the implementation on the first-generation versions. We advice you to cease using this function.
{% endhint %}

## From 0.0.4.5 to 0.0.5

This kernel version incorporated changes to the kernel, such as these API changes that occurred:

{% content-ref url="../version-release-notes/v0.0.5.x-series/v0.0.5.0-beta-versions.md" %}
[v0.0.5.0-beta-versions.md](../version-release-notes/v0.0.5.x-series/v0.0.5.0-beta-versions.md)
{% endcontent-ref %}

{% content-ref url="../version-release-notes/v0.0.5.x-series/" %}
[v0.0.5.x-series](../version-release-notes/v0.0.5.x-series/)
{% endcontent-ref %}

### `DiscoSystem()`

{% code title="GetCommand.vb" lineNumbers="true" %}
```visual-basic
Sub DiscoSystem(Optional ByVal BlackWhite As Boolean = False)
```
{% endcode %}

The very first screensaver ever implemented in the very first version of Nitrocid KS. However, it was implemented in the `DiscoSystem()` function which, unfortunately, situated in the `GetCommand` module, which was a wrong place to start with. It was removed in 0.0.5.

{% hint style="danger" %}
This screensaver was later implemented in later versions of Nitrocid KS as one of the genuine screensavers under the name of Disco. We still advice you to stop using this function.
{% endhint %}

### `BeepFreq()` and `BeepSystem()`

{% code title="Beep.vb" lineNumbers="true" %}
```visual-basic
Sub BeepFreq()
Sub BeepSystem()
```
{% endcode %}

This function served as the prompt handler for the beep system. Actually, this is the Stage 1 function for prompting the user to provide the value of the frequency in hertz. The below function, `BeepSystem()`, is the second-stage function that prompts the user for the duration of the beep in seconds. Both of these functions are removed as part of the mass prompt obsoletion movement that started in the middle of the revision.

{% hint style="info" %}
Though the `beep` command returned without frequency and time support in later revisions, these functions are removed. The prompt obsoletion movement was something we regret making thanks to the evolution of the kernel as revisions come and go.

The reason that the `beep` command returned without the beeping customizability is that because it's not cross-platform.
{% endhint %}

### `CheckNetworkKernel()`, `CheckNetworkCommand()`, and `PingTargetKernel()`

{% code title="Network.vb" lineNumbers="true" %}
```visual-basic
Sub CheckNetworkKernel()
Sub CheckNetworkCommand()
Sub PingTargetKernel(ByVal Address As String)
```
{% endcode %}

These functions come as a duo, presenting different ways to do just one job: pinging the target. However, each function calls their sibling function, `PingTargetKernel()` and `PingTarget()`, the latter of which is not removed. They're removed as a completely unnecessary kernel argument was removed with it and as part of the prompt obsoletion movement.

{% hint style="info" %}
This function was returned much later as a cross-platform method to ping network devices in the `NetworkTools.PingAddress()` function. The below code block can be used to show you the method signature of the function in the latest kernel version.

{% code title="NetworkTools.cs" lineNumbers="true" %}
```csharp
public static PingReply PingAddress(string Address)
public static PingReply PingAddress(string Address, int Timeout)
public static PingReply PingAddress(string Address, int Timeout, byte[] Buffer)
```
{% endcode %}
{% endhint %}

### `panicPrompt()`

{% code title="PanicSim.vb" lineNumbers="true" %}
```visual-basic
Sub panicPrompt()
```
{% endcode %}

This was implemented as the normal command that you can easily run from the shell. As this was declared as abusive, we've removed the function.

{% hint style="info" %}
This function was later re-implemented as a test facade, which, along with the entire kernel testing system, is only available in debug builds of the kernel. Either way, we've internalized the function responsible for the kernel error.
{% endhint %}

### `changeName()`

{% code title="UserManagement.vb" lineNumbers="true" %}
```visual-basic
Sub changeName()
```
{% endcode %}

This function was used to change the username to another name using prompts, but that function was removed as a result of the prompt obsoletion movement.

{% hint style="info" %}
This function made its own return under `ChangeUsername()` in the same module. The code block below shows you the method signature of the function.

{% code title="UserManagement.cs" lineNumbers="true" %}
```csharp
public static void ChangeUsername(string OldName, string Username)
```
{% endcode %}
{% endhint %}

### `changePassword()` and `changePasswordPrompt()`

{% code title="UserManagement.vb" lineNumbers="true" %}
```visual-basic
Sub changePassword()
Sub changePasswordPrompt(ByVal usernamerequestedChange As String)
```
{% endcode %}

Both functions prompt for the same thing, which is changing the target user password. These functions are confusingly named, because the first one prompted for the username whose password will be changed, and the second function prompted for the new password to set for the user. Both of the functions are removed as part of the prompt obsoletion movement.

{% hint style="info" %}
Password changing is returned in later versions of the kernel starting with 0.0.12 under the `ChangePassword()` function. Below code block shows you the method signature for the new function.

{% code title="UserManagement.cs" lineNumbers="true" %}
```csharp
public static void ChangePassword(string Target, string CurrentPass, string NewPass)
```
{% endcode %}
{% endhint %}

### `removeUser()`

{% code title="UserManagement.vb" lineNumbers="true" %}
```visual-basic
Sub removeUser()
```
{% endcode %}

This function used to prompt you for the user which was to be removed. However, it was removed as part of the prompt obsoletion movement.

{% hint style="info" %}
This function was later returned. The below code block shows you the method signature.

{% code title="UserManagement.cs" lineNumbers="true" %}
```csharp
public static void RemoveUser(string user)
```
{% endcode %}
{% endhint %}

### `addUser()` and `newPassword()`

{% code title="UserManagement.vb" lineNumbers="true" %}
```visual-basic
Sub addUser()
Sub newPassword(ByVal user As String)
```
{% endcode %}

The first function used to prompt you for the new user, which, after being provided the new user to create, the second function was called to prompt you for the password of the new user. Both functions are removed as part of the prompt obsoletion movement.

{% hint style="info" %}
`AddUser()` was later implemented in the latest kernel versions. The below code block shows you the usage of the routine.

{% code title="UserManagement.cs" lineNumbers="true" %}
```csharp
public static void AddUser(string newUser, string newPassword = "")
```
{% endcode %}
{% endhint %}

### `UseDefaults()`, `SetColorSteps()`, and `advanceStep()`

{% code title="ColorSet.vb" lineNumbers="true" %}
```visual-basic
Sub UseDefaults()
Sub SetColorSteps()
Sub advanceStep()
```
{% endcode %}

The first function was called by `SetColorSteps()` to get the default values of the kernel template from the number in the global variable that was incremented by the `SetColorSteps()` function itself.

The 2021 analysis of the removed functions says that it was removed as part of the prompt obsoletion movement, but it was actually removed as improvements waded in to the kernel coloring system. This latest analysis is true for both the `UseDefaults()` and `advanceStep()` functions, but the second one was removed for the prompt removal reason.

{% hint style="info" %}
We advice you to use `PopulateColorsDefault()` implemented in 0.1.0 Beta 1 as an alternative to the first function. For the second one, use the `SetConsoleColor()` function found under `KernelColorTools` as shown in the method signature in the below code block.

{% code title="ColorTools.cs" lineNumbers="true" %}
```csharp
public static void SetConsoleColor(KernelColorType colorType)
public static void SetConsoleColor(KernelColorType colorType, bool Background, bool ForceSet = false)
public static void SetConsoleColor(Color ColorSequence, bool Background = false, bool ForceSet = false)
```
{% endcode %}
{% endhint %}

### `TemplatePrompt()`

{% code title="TemplateSet.vb" lineNumbers="true" %}
```visual-basic
Sub TemplatePrompt()
```
{% endcode %}

This function used to prompt you for writing the template name to set the kernel colors to match the selected template. It was removed as part of the prompt obsoletion movement.

{% hint style="info" %}
Use `SetColorsTheme()`, `ApplyThemeFromFile()`, or `ApplyThemeFromResources()` as viable functions to set your theme. The below code block shows you the method signature. Also, the `themesel` command shows you the list of themes and allows you to choose between available themes.

{% code title="ThemeTools.cs" lineNumbers="true" %}
```csharp
public static void SetColorsTheme(ThemeInfo ThemeInfo)
public static void ApplyThemeFromFile(string ThemeFile)
public static void ApplyThemeFromResources(string theme)
```
{% endcode %}
{% endhint %}

## From 0.0.5 to 0.0.5.1

This release was a minor release to the 0.0.5.x series.

### `permissionPrompt()` and `permissionEditingPrompt()`

{% code title="Groups.vb" lineNumbers="true" %}
```visual-basic
Sub permissionPrompt()
Sub permissionEditingPrompt()
```
{% endcode %}

The first function served as the prompt for adding and removing groups for users, and the second function prompted you for editing the groups. Both of these functions were later removed as part of the prompt obsoletion movement.

{% hint style="info" %}
The new permission system replaces the two functions with more API-friendly functions that can be found in the `PermissionsTools` class. However, if you're referring to actual user groups, you may want to take a look at the `GroupManagement` class instead.
{% endhint %}

## From 0.0.5.1 to 0.0.6

0.0.6 brought several features, including the API changes described below:

{% content-ref url="../version-release-notes/v0.0.6.x-series/v0.0.6.0-beta-versions.md" %}
[v0.0.6.0-beta-versions.md](../version-release-notes/v0.0.6.x-series/v0.0.6.0-beta-versions.md)
{% endcontent-ref %}

{% content-ref url="../version-release-notes/v0.0.6.x-series/" %}
[v0.0.6.x-series](../version-release-notes/v0.0.6.x-series/)
{% endcontent-ref %}

### `initializeMainUsers()`

{% code title="UserManagement.vb" lineNumbers="true" %}
```visual-basic
Sub initializeMainUsers()
```
{% endcode %}

This function usused to add the master system account for the kernel known as "root", a familiar name for the superuser account in Linux systems. It was found to be duplicating what `adduser()` does in the same version of the kernel, so we decided to merge it to `adduser()`.

The 2021 analysis said that it was removed as it's no longer needed, but it pointed to `Login.vb` where no such function exists.

{% hint style="danger" %}
Even though this function returned in later versions of the kernel as `InitializeSystemAccount()` in 0.0.12, we advice you to cease using this function.
{% endhint %}

### `ReadLineWithNewLine()`

{% code title="StreamReaderExtensions.vb" lineNumbers="true" %}
```visual-basic
Public Function ReadLineWithNewLine(ByVal reader As StreamReader) As String
```
{% endcode %}

This StreamReader extension used to read one line ending in the new line character from the file stream reader. It got removed as part of the migration to Extensification, which is our (obsolete) project dedicated to giving you additional extensions.

{% hint style="danger" %}
We advice you to cease using this function. The codebase to implement this function is still found in the archived Extensification repository, but we advice you to re-implement this functionality yourself.
{% endhint %}

### `ReadyPath_MOD()`

{% code title="ModParser.vb" lineNumbers="true" %}
```visual-basic
Sub ReadyPath_MOD()
```
{% endcode %}

This function was implemented as part of the addition of Unix systems to the supported host systems. It was later removed as unnecessary.

{% hint style="info" %}
If you want to get a path to your mod directory, use the `ModsPath` property found in the Paths module. Alternatively, you can use a function to get said kernel path with the `GetKernelPath` function if you pass it the `KernelPathType.Mods` value.

{% code title="Paths.cs" lineNumbers="true" %}
```csharp
public static string ModsPath
public static string GetKernelPath(KernelPathType PathType)
```
{% endcode %}
{% endhint %}

### `ProbeGPU()`, `Hddinfo()`, `Cpuinfo()`,  `SysMemory()`, and `BiosInformation()`

{% code title="HardwareProbe.vb" lineNumbers="true" %}
```visual-basic
Public Sub ProbeGPU(Optional ByVal KernelMode As Boolean = True, Optional ByVal QuietMode As Boolean = False)
Public Sub Hddinfo(Optional ByVal QuietMode As Boolean = False)
Public Sub Cpuinfo()
Public Sub SysMemory(Optional ByVal QuietMode As Boolean = False)
Public Sub BiosInformation()
```
{% endcode %}

These hardware parsing functions used to probe information about your computer's graphics card, hard drive (including SSDs), CPU, memory, and BIOS. These functions have since been removed as a merger to `ProbeHardware()`, but it got removed again after Inxi.NET got released.

{% hint style="info" %}
`ListHardware` is implemented to show you info about hardware based on supported types, or "all" to show everything. The method signature shows you how you can use this method defined in `HardwareList`.

{% code title="HardwareList.cs" lineNumbers="true" %}
```csharp
public static void ListHardware(string HardwareType)
```
{% endcode %}
{% endhint %}

### `PreWriteToDebugger`, `PostWriteToDebugger`, `PreWriteToConsole`, and `PostWriteToConsole` events

{% code title="EventsAndExceptions.vb" lineNumbers="true" %}
```visual-basic
' Events
<Obsolete> Public Event PreWriteToDebugger() 'Writing event - Deprecated
<Obsolete> Public Event PostWriteToDebugger() 'Writing event - Deprecated
<Obsolete> Public Event PreWriteToConsole() 'Writing event - Deprecated
<Obsolete> Public Event PostWriteToConsole() 'Writing event - Deprecated

' Responders
<Obsolete> Public Sub RespondPreWriteToDebugger() Handles Me.PreWriteToDebugger
<Obsolete> Public Sub RespondPostWriteToDebugger() Handles Me.PostWriteToDebugger
<Obsolete> Public Sub RespondPreWriteToConsole() Handles Me.PreWriteToConsole
<Obsolete> Public Sub RespondPostWriteToConsole() Handles Me.PostWriteToConsole

' Raisers
<Obsolete> Public Sub RaisePreWriteToDebugger()
<Obsolete> Public Sub RaisePostWriteToDebugger()
<Obsolete> Public Sub RaisePreWriteToConsole()
<Obsolete> Public Sub RaisePostWriteToConsole()
```
{% endcode %}

These events were raised if the conditions were met:

* If any console writer tried to write something to the console, the `PreWriteToConsole` event was raised before it's written. After performing the operation, `PostWriteToConsole` was raised.
* If the debug writer tried to write debug info to the debug file when the kernel debugger is on, the `PreWriteToDebugger` event was raised before it's written. After that, `PostWriteToDebugger` event was raised.

Our main issue was that performing this operation caused us to significantly slow down the performance, so they remained unused and given the Obsolete attribute before getting removed.

{% hint style="danger" %}
There is no alternative to these events, so we recommend you to avoid using these events in your mods if possible. This is an upgrade block for your mods until you follow our advice.
{% endhint %}

### `ResetUsers()`

{% code title="UserManagement.vb" lineNumbers="true" %}
```visual-basic
Public Sub resetUsers()
```
{% endcode %}

This function used to reset every variable related to user management, including the outputs and the inputs. It got merged to `ResetEverything()`

{% hint style="danger" %}
We advice you to cease using this function.
{% endhint %}

### `GetAllCurrencies()`

{% code title="unitConv.vb" lineNumbers="true" %}
```visual-basic
Public Function GetAllCurrencies() As List(Of currencyInfo)
```
{% endcode %}

This function used to list all the currencies of all the countries supported by the `free.currencyconverterapi.com` API site. However, the operators behind it converted the API to paid model, so we removed it following that change.

{% hint style="danger" %}
We no longer support currency conversions as part of the current state of the economy, including the recent inflations. We advice you to cease using this feature if possible.
{% endhint %}

### `CurrencyConvert()`

{% code title="unitConv.vb" lineNumbers="true" %}
```visual-basic
Sub CurrencyConvert(ByVal src3UPchars As String, ByVal dest3UPchars As String, ByVal value As Double)
```
{% endcode %}

This function used to query the aforementioned API to convert the value from the source country currency to the target country currency. It got removed when the `free.currencyconverterapi.com` operators moved to the paid-only model.

{% hint style="danger" %}
We no longer support currency conversions as part of the current state of the economy, including the recent inflations. We advice you to cease using this feature if possible.
{% endhint %}

## From 0.0.6 to 0.0.7

This version series is the last version group for Nitrocid KS API v1.0, which incorporated many changes, such as the addition of languages. In contrast, the following APIs are removed:

{% content-ref url="../version-release-notes/v0.0.7.x-series/v0.0.7.0-beta-versions.md" %}
[v0.0.7.0-beta-versions.md](../version-release-notes/v0.0.7.x-series/v0.0.7.0-beta-versions.md)
{% endcontent-ref %}

{% content-ref url="../version-release-notes/v0.0.7.x-series/" %}
[v0.0.7.x-series](../version-release-notes/v0.0.7.x-series/)
{% endcontent-ref %}

### `ExpressionCalculate()`

```visual-basic
'stdCalc.vb
Public Sub expressionCalculate(ByVal ParamArray exps() As String)

'sciCalc.vb
Public Sub expressionCalculate(ByVal sciMode As Boolean, ByVal ParamArray exps() As Object)
```

For the standard calculator, it calculates all the expressions that consist of just numbers and operators and prints their results to the console. It takes the expression list and lists all numbers and operators in their separate lists and combines them to the final expression string to be computed by the DataTable class which can be found by clicking below:

{% embed url="https://learn.microsoft.com/en-us/dotnet/api/system.data.datatable.compute?view=net-6.0" %}

In the other hand, the scientific calculator does the same thing, but also checks to see if it's running in sin-cos-tan mode (also wrongly named as scientific mode).

These functions were removed because their support for expressions are extremely limited and the implementation is fragile, as it doesn't check for parentheses, powers, and so on.

{% hint style="info" %}
This function later returned with the usage of `StringMath` in the later kernel versions. Before the migration, in 0.0.12, it has returned under `DoCalc()`.
{% endhint %}

### `Converter()`

{% code title="unitConv.vb" lineNumbers="true" %}
```visual-basic
Public Sub Converter(ByVal sourceUnit As String, ByVal targetUnit As String, ByVal value As Object)
```
{% endcode %}

This converter used to take both the source unit and the target unit to convert the value to the target unit. A removal was made as a result of it being hard to maintain.

{% hint style="info" %}
It was later returned with the usage of `Units.NET` library.
{% endhint %}

### `Wln()`

{% code title="TextWriterColor.vb" lineNumbers="true" %}
```visual-basic
Public Sub Wln(ByVal text As Object, ByVal colorType As String, ByVal ParamArray vars() As Object)
```
{% endcode %}

This function was implemented to create a separate version of `W()` (now `Write()`) that also prints the new line after writing the needed string to the console. It was removed as it was being merged by `W()`.

{% hint style="info" %}
Use the `lines` boolean parameter to make `Write()` make a new line after it writes down the needed text.
{% endhint %}

### `ReadImportantConfig()`

{% code title="Config.vb" lineNumbers="true" %}
```visual-basic
Public Sub readImportantConfig()
```
{% endcode %}

ThThis function was used by the config initialization routine to read configuration entries that were deemed as important. This, however, was merged to the main config reader where it was given substantial updates before eventually getting removed in 0.1.0 as part of the new config reader and writer.

{% hint style="danger" %}
We advice you to cease using this function.
{% endhint %}

### `GenModCS()`

{% code title="ModParser.vb" lineNumbers="true" %}
```visual-basic
Private Function GenModCS(ByVal code As String) As IScript 'For C# Mods
```
{% endcode %}

The modding system first saw support for C# in 0.0.6.2, so this function was implemented separately. However, it got merged to GenMod() with this version series.

{% hint style="info" %}
This function was not part of the public API, but listed here according to the 2021 analysis. The modification system in the latest series now supports .DLL files, but withdraws support for a single .cs and .vb mods.
{% endhint %}

### Manual Page API

{% code title="Manual.vb" lineNumbers="true" %}
```visual-basic
' Manual class
Public Class Manual
```
{% endcode %}

{% code title="PageParser.vb" lineNumbers="true" %}
```visual-basic
'Initializes manual pages
Sub InitMan()

'Checks for manual page if it's valid
Public Sub CheckManual(ByVal Title As String)

'Check for any TODO
Public Sub CheckTODO(ByVal line As String)

'Parse manual file from KS, not mods
Public Sub ParseMan_INTERNAL(ByVal line As String)

'Parse manual file from mods (Not implemented yet)
Public Sub ParseMan_EXTERNAL(ByVal line As String, ByVal ManFile As String)

'Get strings until end of body
Public Sub ParseBody(ByVal line As String)

'The colors on the manpage will be parsed
Public Sub ParseColor(ByVal line As String)

'Parse sections
Public Sub ParseSection(ByVal line As String)

'Perform a sanity check on internal manpages
Public Sub Sanity_INTERNAL(ByVal title As String)

'Perform a sanity check on mod manpages (Not implemented)
Public Sub Sanity_EXTERNAL(ByVal title As String)
```
{% endcode %}

{% code title="PageViewer.vb" lineNumbers="true" %}
```visual-basic
'Preview the manual page
Public Sub ViewPage(ByVal title As String)

'Writes page info
Private Sub WriteInfo(ByVal title As String)
```
{% endcode %}

The manual page for Nitrocid KS was first situated in the application itself using an in-house format known as the `.ksman` extension. It was the start of the actual product documentation, until several display issues on the manual API surfaced with the release, especially on Linux systems. We had to make several fixes before abandoning the entire viewer.

The parser, however, works well. We recently had time issues with translating pretty much every manual page to the point that the manual page count was very high. We felt like that it was a very high volume, so we decided to remove them, and migrate the manual to GitHub Wiki.

{% hint style="info" %}
The manual page feature made its own return for 0.0.20, but it only works for mods and not the actual user guide for Nitrocid KS, which is what you're reading here.

To learn more about how manual pages work and how to use them for your mods, press the below link:

[#manual-page-parsing](../../advanced-and-power-users/kernel-modifications/kernel-modification-management.md#manual-page-parsing "mention")
{% endhint %}

### `ListLocal()`

{% code title="FTPGetCommand.vb" lineNumbers="true" %}
```visual-basic
Public Sub ListLocal(ByVal dir As String)
```
{% endcode %}

This function was a helper function to list all local directories, but specialized for the FTP shell that first appeared in 0.0.5.5. It was later removed because it was duplicating what `List()` was doing.

{% hint style="danger" %}
We advise you to cease using this function, and start using the `List()` function. The method signature for this function is listed below.

```csharp
// Get a list of files and directories straight to the terminal
public static void List(string folder)
public static void List(string folder, bool Sort)
public static void List(string folder, bool ShowFileDetails, bool SuppressUnauthorizedMessage)
public static void List(string folder, bool ShowFileDetails, bool SuppressUnauthorizedMessage, bool Sort, bool Recursive = false)

// Get a list of files and directories and manipulate them programmatically
public static List<FileSystemInfo> CreateList(string folder, bool Sorted = false, bool Recursive = false)
```
{% endhint %}

### `PingTarget()`

{% code title="NetworkTools.vb" lineNumbers="true" %}
```visual-basic
Public Sub PingTarget(ByVal Address As String, Optional ByVal repeatTimes As Int16 = 3)
```
{% endcode %}

This function was used to ping a remote computer or remote server across the LAN and across the entire Internet. It was removed because it was functionally using the `My.Computer` API which wasn't cross-platform and was restricted to Visual Basic, which defeats the purpose of cross-platform applications.

{% hint style="info" %}
This function was returned much later as a cross-platform method to ping network devices in the `NetworkTools.PingAddress()` function. The below code block can be used to show you the method signature of the function in the latest kernel version.

{% code title="NetworkTools.cs" lineNumbers="true" %}
```csharp
public static PingReply PingAddress(string Address)
public static PingReply PingAddress(string Address, int Timeout)
public static PingReply PingAddress(string Address, int Timeout, byte[] Buffer)
```
{% endcode %}
{% endhint %}

### Network Listing API

{% code title="NetworkTools.vb" lineNumbers="true" %}
```visual-basic
Public Sub ListOnlineAndOfflineHosts()
Public Sub ListHostsInNetwork()
Public Sub GetNetworkComputers()
Public Sub ListHostsInTree()
```
{% endcode %}

These four functions were made to list all the computers in the LAN. The first one lists all the offline and online hosts by taking cached computer list, the second and the fourth one lists all the online computers either by listing them or by putting them under the tree-based diagram, and the third function actually uses SAMBA 1.0, which is highly insecure considering that there are many security vulnerabilities regarding this version of SAMBA which is linked below.

{% embed url="https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=smbv1" %}
All SAMBA 1.0 vulnerabilities
{% endembed %}

As a consequence, we decided to remove the whole API, making the kernel non-dependent on SAMBA 1.0.

{% hint style="info" %}
It was later returned in 0.1.0 pre-beta, but it doesn't use the insecure SAMBA 1.0 protocol. Instead, it uses ICMP pinging to check for all online devices in the LAN. You can use this function as listed below.

{% code title="NetworkTools.cs" lineNumbers="true" %}
```csharp
public static IPAddress[] GetOnlineDevicesInNetwork()
```
{% endcode %}
{% endhint %}
