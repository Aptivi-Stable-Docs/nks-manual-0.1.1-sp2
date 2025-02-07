---
description: Guide for upgrading 0.1.0 Beta 2 mods to 0.1.0 Beta 3
---

# ⬆ From 0.1.0 Beta 2 to 0.1.0 Beta 3

This page lists all the changes that have been made from 0.1.0 Beta 2 to 0.1.0 Beta 3. For upgrading your mods from 0.0.24.x directly to the 0.1.0 series, use the main upgrade page instead.

### Migrated to `ProvidedArgumentsInfo`

```csharp
public class ProvidedCommandArgumentsInfo
public class ProvidedArgumentArgumentsInfo
```

The first class provided information about the command arguments. It held information about user input, and it indicated whether the user input was correct or not. It worked on the UESH shell and its sub-shells either implemented by ourselves or by a custom mod. The second class, `ProvidedArgumentArgumentsInfo`, did exactly the same thing, but with one difference: it worked with kernel arguments.

However, this was considered code repetition as 95% of the parts were repeated with differences in names, so we decided to refactor these two classes to a single `ProvidedArgumentsInfo` with the following public functions found in `ArgumentsParser`:

* `ParseShellCommandArguments()`
* `ParseArgumentArguments()`

{% hint style="info" %}
We advice you to replace all calls to `Provided*ArgumentsInfo` constructors and start using the `Parse*Arguments()` function found in `ArgumentsParser`, which does the very job of parsing commands and kernel arguments.
{% endhint %}

### Migrated TUI apps' colors to `TuiColors`

We used to provide users options to change each built-in TUI application's colors, like the file manager, the contacts manager, and the task manager. Sadly, we had to migrate all these configuration entries to a single configurable TUI color class, `TuiColors`.

{% hint style="info" %}
Currently, there is no plan that proposes restoring this.
{% endhint %}

### Moved `KernelErrorLevel` to `Kernel.Exceptions`

```csharp
public enum KernelErrorLevel
```

`KernelErrorLevel` was used by the kernel panic module, a group of functions that get executed when there was a kernel error, depending on the severity of the error and the state of the kernel.

Looking at the structure, we saw that it was left alone in the process of migrating all kernel error-related to the Kernel.Exceptions namespace, so we decided to finish the merge process by relocating `KernelErrorLevel` to `Kernel.Exceptions`.

{% hint style="warning" %}
Although the KernelErrorLevel enumeration is public, the kernel error functions are internal, so we may remove visibility from the public API at some point during the Beta 3 development.
{% endhint %}

### Aliases and usability problems

```csharp
public static bool AddAlias(string SourceAlias, string Destination, string Type)
public static bool AddAlias(string SourceAlias, string Destination, ShellType Type)
```

Aliases were first implemented as hard-coded aliases back on 0.0.1, but it got evolved into user-configurable aliases and went well with no problems.

However, what we haven't noticed during the custom shell type implementation is that the `ShellType` version of `AddAlias()` tends to repeat itself, causing the stack to overflow and Nitrocid KS to crash on every function that called it. This affected all the versions that implemented the custom shell types; all the way to 0.1.0 Beta 2.

However, we've noticed that the alias creation logic treats the source (a command that will be aliased to) as a target, and the target (an alias name) as a source, so we've decided to make some breaking changes to correct this confusion.

{% hint style="warning" %}
We can't document these APIs as a result of this confusion until we rewrite them with care. This re-write will reduce confusion between the source and the target, and ensure that aliases are added properly and that the tests are reporting success.
{% endhint %}

### Screensaver probing changed

```csharp
public class CustomSaverInfo
public static class CustomSaverParser
public static void InitializeCustomSaverSettings()
public static void SaveCustomSaverSettings()
public static void AddCustomSaverToSettings()
public static void RemoveCustomSaverFromSettings()
public static object GetCustomSaverSettings()
public static bool SetCustomSaverSettings()
Dictionary<string, object> ScreensaverSettings { get; set; }
```

Screensavers used to be found in their own path, called `KSScreensavers`, usually found in your user's local app data folder. However, this was implemented at a time that mods were still one-line C# and Visual Basic code files and didn't support dependencies.

When we started improving the mod parsing code, we've made these improvements starting from a version that implemented kernel modding until now, and these improvements were still getting committed.

This caused us to ditch the following features:

* Custom screensaver settings, as they have never been tested with the latest version of Nitrocid. The only time that we've tested this feature was when we were implementing this feature, and that was a long time ago.
* Custom screensaver parser that worked exactly like mod parsing code was removed and replaced with the register and unregister logics, since they were better in various degrees. This was done as a result of recent improvements to the modding system.
* `CustomSaverInfo` was removed, as none of its properties, except the base screensaver, looked like it had any use anywhere in the entire screensaver management and kernel mod management functions.
* `ReloadSaver` was removed from the list of available commands as a result of this breaking change. You can now use the kernel mod manager to load an updated version of any mod, which may register screensavers on boot.
* Paths to custom screensaver and its settings were removed from the available kernel path list as a result of all the above removals.

{% hint style="info" %}
In order to be able to use a screensaver provided by your mods, they must now call the following functions from `CustomSaverTools`:

* When the mod starts up (`StartMod()`), they must call the `RegisterCustomScreensaver()` function, pointing it to your new instance of your base screensaver class.
* When the mod shuts down (`StopMod()`), they must call the `UnregisterCustomScreensaver()` function, pointing it to the name of the screensaver that you want to unregister.

You can check to see if your screensaver is initialized by calling the `IsScreensaverRegistered()` function.
{% endhint %}

### Screensaver management class renamed

{% code title="Screensaver.cs" lineNumbers="true" %}
```csharp
public static class Screensaver
```
{% endcode %}

When we first implemented the screensavers, we used to call the class that was responsible for the management of the screensavers, `Screensaver`. However, it was done when Visual Basic was the dominant language for the whole simulator.

As a result of merging to C# on Milestone 2 of 0.1.0, we've decided to make final migration changes by renaming the screensaver management class to `ScreensaverManager`.

{% hint style="info" %}
We recommend that you change all references to `ScreensaverManager` when migrating from 0.1.0 Beta 2 or lower.
{% endhint %}

### SSH class renamed

{% code title="SSH.cs" lineNumbers="true" %}
```csharp
public static class SSH
```
{% endcode %}

When we first implemented the SSH shell, we used to call the class that was responsible for launching the SSH shell, `SSH`. However, it was done when Visual Basic was the dominant language for the whole simulator.

As a result of merging to C# on Milestone 2 of 0.1.0, we've decided to make final migration changes by renaming this class to `SSHTools`.

{% hint style="info" %}
We recommend that you change all references to `SSHTools` when migrating from 0.1.0 Beta 2 or lower.
{% endhint %}

### Speed Dials and Network Connections

{% code lineNumbers="true" %}
```csharp
// FTPTools.cs and SFTPTools.cs
public static void QuickConnect()
public static void SFTPQuickConnect()

// SpeedDialTools.cs
public static void AddEntryToSpeedDial(string Address, int Port, SpeedDialType SpeedDialType, bool ThrowException = true, params object[] arguments)
public static bool TryAddEntryToSpeedDial(string Address, int Port, SpeedDialType SpeedDialType, bool ThrowException = true, params object[] arguments)

// SpeedDialType.cs
public enum SpeedDialType
```
{% endcode %}

When network connections were introduced in Beta 2, we didn't have an opportunity to make speed dials interact properly with network connections.

After Beta 2 was released, we finally took action to remove `SpeedDialType` so that `NetworkConnectionType` can be used instead. As a result, `QuickConnect()` functions in the FTP and SFTP tools code were gone and `AddEntryToSpeedDial()` and its `Try` function had its speed dial type argument replaced with `NetworkConnectionType`.

{% hint style="info" %}
Replace all calls to `SpeedDialType` with `NetworkConnectionType`. For custom network connection types, you can use new overloads of the two `Add` functions, which take a string representation of your custom network connection type.
{% endhint %}

### Handling multiple `CommandArgumentInfo` instances

```csharp
// For arguments
public ArgumentInfo(string Argument, string HelpDefinition, CommandArgumentInfo ArgArgumentInfo, ArgumentExecutor ArgumentBase, bool Obsolete = false, Action AdditionalHelpAction = null)

// For commands
public CommandInfo(string Command, ShellType Type, string HelpDefinition, CommandArgumentInfo CommandArgumentInfo, BaseCommand CommandBase, CommandFlags Flags = CommandFlags.None)
public CommandInfo(string Command, string Type, string HelpDefinition, CommandArgumentInfo CommandArgumentInfo, BaseCommand CommandBase, CommandFlags Flags = CommandFlags.None)
```

To add support for multiple `CommandArgumentInfo` instances in one `CommandInfo` or `ArgumentInfo`, we now had to replace all single `CommandArgumentInfo` parameters in the above constructors with an array argument of the same class.

This resulted in us having to adjust the entire shell system to adapt to this kind of change.

{% hint style="info" %}
Your mods must now follow the new pattern of how to define new `CommandInfo` instances in the `Commands` property in your shell class, provided that you've committed the below changes. The page below explains in depth about how to define these instances.

[shell-structure](../../../advanced-and-power-users/inner-workings/shell-structure/ "mention")

The following parameters were changed:

* `CommandInfo`
  * Before: `CommandArgumentInfo CommandArgumentInfo`
  * After: `CommandArgumentInfo[] CommandArgumentInfo`
* `ArgumentInfo`
  * Before: `CommandArgumentInfo ArgArgumentInfo`
  * After: `CommandArgumentInfo[] ArgArgumentInfo`
{% endhint %}

### Screensaver displayer class is no longer public

{% code title="ScreensaverDisplayer.cs" lineNumbers="true" %}
```csharp
public static class ScreensaverDisplayer
public static void DisplayScreensaver(BaseScreensaver Screensaver)
```
{% endcode %}

ScreensaverDisplayer was created as a way to show screensavers efficiently when called with `ShowSavers()`. Upon further inspection, `DisplayScreensaver()` behaves exactly the same as the `ShowSavers()` function, but with more relaxed flagging and checking.

This function was used as a thread handler, but it looks like that it can be called from any mod, so we decided to demote the class and the method shown above so that mods can't use them.

{% hint style="danger" %}
It's recommended to cease using the above class and start using `ShowSavers()` as the only suitable alternative.
{% endhint %}

### Error codes and `-set` unified switch

{% code title="BaseCommand.cs" lineNumbers="true" %}
```csharp
public virtual void Execute(string StringArgs, string[] ListArgsOnly, string[] ListSwitchesOnly)
```
{% endcode %}

Initially, on Beta 2, it had a very basic error code support that set a UESH variable called `UESHErrorCode`. It didn't support errors which came from the commands itself; only from the command processor, `GetLine()`.

However, we decided to implement the following features:

* Improved error codes: UESH scripts can now rely on error codes more reliably, because error codes can now be get straight from the base command implementation, `Execute()` to be specific.
* \-set unified switch: For commands that set their output to a specific UESH variable, this switch will set that variable to the generated output, provided that the command already sets the output value, which is `ref string variableValue`.

As a result, this massive breaking change had to be done in order to implement the two abovementioned features.

{% hint style="info" %}
Please note that there are no built-in commands which make use of the second feature, but your mods could implement them in their own commands.

Your mods and its commands, however, will have to adapt to the first feature by changing the signature shown at the top of this section to the below signature:

```csharp
public virtual int Execute(string StringArgs, string[] ListArgsOnly, string[] ListSwitchesOnly, ref string variableValue)
```
{% endhint %}

### Moved notification priority and type enumerations

{% code title="NotificationManager.cs" lineNumbers="true" %}
```csharp
public enum NotifPriority
public enum NotifType
```
{% endcode %}

We've moved the two above enumerations, `NotifPriority` and `NotifType`, and renamed them to their extended names in their own code files. This is to make referencing both of them easier when creating notification instances and managing them.

{% hint style="info" %}
You no longer have to reference NotificationManager before one of the two above enumerations. However, you'll have to retry referencing them under the new names:

* `NotifPriority` -> `NotificationPriority`
* `NotifType` -> `NotificationType`
{% endhint %}

### Commands that don't accept `-set`

{% code title="CommandArgumentInfo.cs" lineNumbers="true" %}
```csharp
public CommandArgumentInfo(string[] Arguments, SwitchInfo[] Switches, bool ArgumentsRequired, int MinimumArguments, Func<string, int, char[], string[]> AutoCompleter = null)
```
{% endcode %}

We've implemented `-set` as a unified switch to set the variable to the value of the command output generated by the command class. However, there are commands that don't use this feature, hence we no longer accept their usage of `-set` unless absolutely necessary.

{% hint style="info" %}
You need to take the following cases into account:

* If your command doesn't use the custom completion and your command doesn't use the `-set` switch, there is no action to be taken.
* If your command doesn't use the custom completion, but your command uses the `-set` switch, you must set the `AcceptsSet` parameter to `true`.
* If your command uses the custom completion, you may specify whether the command accepts or declines the `-set` switch, but you must enter this value before the command completion argument. The signature will help illustrate this:

```csharp
public CommandArgumentInfo(string[] Arguments, SwitchInfo[] Switches, bool ArgumentsRequired, int MinimumArguments, bool AcceptsSet = false, Func<string, int, char[], string[]> AutoCompleter = null)
```
{% endhint %}

### Used SemanVer on `KernelUpdate`

{% code title="KernelUpdate.cs" lineNumbers="true" %}
```csharp
public Version UpdateVersion { get; private set; }
```
{% endcode %}

As SemanVer was released to the public, we decided to release a new version of SemanVer handling SemVer 2.0-compliant versions that also follow the four-part versioning so that Nitrocid KS can accurately check for updates.

As a result, we no longer have to deal with conflicts, like debates about 0.1.0 Beta 1 being the "final" version for 0.0.24.x or lower, which is going to take effect on these series' Backport Fridays.

{% hint style="info" %}
We advice you to adjust your usage of `UpdateVersion` so that it is suitable with the `SemVer` class.
{% endhint %}

### Removed `KernelUpdateInfo`

{% code title="KernelUpdateInfo.cs" lineNumbers="true" %}
```csharp
public class KernelUpdateInfo
```
{% endcode %}

The kernel updater used to host two classes:

* `KernelUpdate` to fetch the updates and install relevant information to the class, like the version and the update link, and
* `KernelUpdateInfo` to host these two again.

However, `KernelUpdateInfo` was proving its redundancy regarding its functionality, so we've removed it.

{% hint style="info" %}
We advice you to use `KernelUpdate` instead of `KernelUpdateInfo`.
{% endhint %}

### Added `FileSystemEntry`

{% code title="BaseFilesystemDriver.cs (implements IFilesystemDriver)" lineNumbers="true" %}
```csharp
public virtual List<FileSystemInfo> CreateList(string folder, bool Sorted = false, bool Recursive = false)
public virtual void PrintDirectoryInfo(FileSystemInfo DirectoryInfo)
public virtual void PrintDirectoryInfo(FileSystemInfo DirectoryInfo, bool ShowDirectoryDetails)
public virtual void PrintFileInfo(FileSystemInfo FileInfo)
public virtual void PrintFileInfo(FileSystemInfo FileInfo, bool ShowFileDetails)
public virtual string SortSelector(FileSystemInfo FileSystemEntry, int MaxLength)
```
{% endcode %}

{% code title="Listing.cs" lineNumbers="true" %}
```csharp
public static List<FileSystemInfo> CreateList(string folder, bool Sorted = false, bool Recursive = false)
```
{% endcode %}

{% code title="DirectoryInfoPrinter.cs" lineNumbers="true" %}
```csharp
public static void PrintDirectoryInfo(FileSystemInfo DirectoryInfo)
public static void PrintDirectoryInfo(FileSystemInfo DirectoryInfo, bool ShowDirectoryDetails)
```
{% endcode %}

{% code title="FileInfoPrinter.cs" lineNumbers="true" %}
```csharp
public static void PrintFileInfo(FileSystemInfo FileInfo)
public static void PrintFileInfo(FileSystemInfo FileInfo, bool ShowFileDetails)
```
{% endcode %}

We used to rely on `FileSystemInfo` to get basic information about any file or folder. However, we needed to extend this class to include more functions to it, so we've implemented `FileSystemEntry`.

As a result, we've replaced every `FileSystemInfo` instances with FileSystemEntry. You can still access this instance through the `BaseEntry` property.

{% hint style="info" %}
We suggest that you change how you call the above functions so that you can use an instance of `FileSystemEntry`. You can create an instance of it using the constructor.
{% endhint %}

### New way of getting a list of themes

{% code title="ThemeTools.cs" lineNumbers="true" %}
```csharp
public readonly static Dictionary<string, ThemeInfo> Themes = new()
```
{% endcode %}

We used to resort to using the above field to get a list of available themes and getting an instance of the selected theme. However, the current approach was reachable to all the kernel mods and can be manipulated with.

Also, we didn't want to add a single theme to three different places, so we decided to just let the kernel populate the themes itself by splitting one massive resources file into three different resource files:

* LanguagesResources
* SettingsResources
* ThemesResources

We then make the above field private, but you can get a copy of it using the `GetInstalledThemes()` function. We've also changed the resource name of the default theme from `_Default` to `Default` as we're no longer in the Visual Basic land.

{% hint style="info" %}
You can get a list of themes using this function, which has the below signature:

```csharp
public static Dictionary<string, ThemeInfo> GetInstalledThemes()
```
{% endhint %}

### Added themes support for interactive TUI colors

{% code title="InteractiveTuiColors.cs" lineNumbers="true" %}
```csharp
public static class InteractiveTuiColors
```
{% endcode %}

When migration of interactive TUI colors was done, the end result was that `InteractiveTuiColors` was made to store all the interactive TUI colors for each element, like the pane background color, selected pane box border color, and so on.

However, we needed to reduce the number of reboots to set these colors, so we decided to remove this class, add these colors to the kernel color type enumeration, and update all themes to use this new feature.

This caused the interactive TUI to maintain color consistency with the rest of the Nitrocid colors.

{% hint style="info" %}
You usually don't have to do anything. You can still access these base colors using the `Config.MainConfig` class. No names are changed.
{% endhint %}

### Added connection options from speed dial

{% code title="NetworkConnectionTools.cs" lineNumbers="true" %}
```csharp
public static void OpenConnectionForShell(ShellType shellType, Func<string, NetworkConnection> establisher, string address = "")
public static void OpenConnectionForShell(string shellType, Func<string, NetworkConnection> establisher, string address = "")
```
{% endcode %}

The network connections didn't support speed dial options, so we decided to add support for it. As a consequence, we've had to change its signature so that it would hold both the normal connection establisher and the speed-dial-based connection establisher.

{% hint style="info" %}
You must adjust your calls to the two above functions so that they also hold the speed-dial-based connection establisher, as indicated in the signatures below:

```csharp
public static void OpenConnectionForShell(ShellType shellType, Func<string, NetworkConnection> establisher, Func<string, JToken, NetworkConnection> speedEstablisher, string address = "")
public static void OpenConnectionForShell(string shellType, Func<string, NetworkConnection> establisher, Func<string, JToken, NetworkConnection> speedEstablisher, string address = "")
```
{% endhint %}

### Changed how event handler registration works

{% code title="IMod.cs" lineNumbers="true" %}
```csharp
void InitEvents(EventType Event);
void InitEvents(EventType Event, params object[] Args);
```
{% endcode %}

InitEvents can be huge because there are too many event types to handle, so we decided to go the easier and the cleaner way and switch the handler from `InitEvents()` to the delegate-based handler.

{% hint style="info" %}
We recommend merging away from `InitEvents()` to use both of the register and unregister functions that are outlined below.

```csharp
public static void RegisterEventHandler(EventType eventType, Action<object[]> eventAction)
public static void UnregisterEventHandler(EventType eventType, Action<object[]> eventAction)
```

Although your mod can contain the `InitEvents()` code because Nitrocid doesn't error out, we still advice you to follow the recommendation for your event handler code to continue working, as well as storing a list of event handlers (at least a `List<Action<object[]>>`) to be able to register and unregister them.
{% endhint %}

### Removed "background trigger"

{% code title="KernelColorTools.cs" lineNumbers="true" %}
```csharp
public static void LoadBack(Color ColorSequence, bool Force = false)
public static void SetConsoleColor(KernelColorType colorType, bool Background, bool ForceSet = false)
public static void SetConsoleColor(Color ColorSequence, bool Background = false, bool ForceSet = false)
```
{% endcode %}

{% code title="Flags.cs" lineNumbers="true" %}
```csharp
public static bool SetBackground =>
    Config.MainConfig.SetBackground;
```
{% endcode %}

This feature was implemented in the early stages of development for the API 3.0 kernel series. However, this wasn't tested for a long time ever since Milestone X was out, so we decided to remove this configuration.

As a result, we've removed the `Force` and the `ForceSet` variables.

{% hint style="danger" %}
We advice you to cease using this function.
{% endhint %}

### Custom command addition changed

{% code title="IMod.cs" lineNumbers="true" %}
```csharp
Dictionary<string, CommandInfo> Commands { get; set; }
```
{% endcode %}

Nitrocid used to use the `Commands` dictionary to register all of the commands during the mod startup. However, this was found to be unnecessary because `StartMod()` was also an entry point code for each mod. Also, this breaking change was made with respect to the recent UESH shell improvements regarding the command handling logic.

So, we decided to remove this key from the interface, making it redundant for general purposes. However, you may choose to keep the `Commands` list, given that you're going to use the command registration functions added with this change and that the commands in the `CommandInfo` instances are the same as the command dictionary keys.

Historically, the `Commands` list was originated when there was no meaningful way to customize the shell and when the command handler was not using the base command classes for each of them.

The below link shows how to use the register/unregister functions.

{% content-ref url="../../../advanced-and-power-users/inner-workings/shell-structure/" %}
[shell-structure](../../../advanced-and-power-users/inner-workings/shell-structure/)
{% endcontent-ref %}

{% hint style="info" %}
Mods that expect commands to be added automatically must now use the `RegisterCustomCommand*` functions on mod start and `UnregisterCustomCommand*` functions on mod shutdown. This allows you to dynamically add commands to the shells without affecting the base shell command list.

However, if you want a simple migration way, don't remove the `Commands` dictionary as it's unused; just use it to get its keys (strings) and values (`CommandInfo` instances) to use `UnregisterCustomCommands()` and `RegisterCustomCommands()` in the shutdown (`StopMod()`) and the startup code (`StartMod()`), respectively.
{% endhint %}

### Added theme addons

{% code title="ThemeInfo.cs" lineNumbers="true" %}
```csharp
public ThemeInfo(string ThemeResourceName)
```
{% endcode %}

The above `ThemeInfo` constructor was made obsolete due to all the themes migrating to the theme addons. This was done to try to reduce the overall size of the main Nitrocid binary.

{% hint style="info" %}
We recommend that you use the output of the `GetInstalledThemes()` function and query a theme info instance from the theme name instead.
{% endhint %}

### Improved the query API for text editor

{% code title="TextEditTools.cs" lineNumbers="true" %}
```csharp
public static Dictionary<int, Dictionary<int, string>> TextEdit_QueryChar(char Char)
public static Dictionary<int, string> TextEdit_QueryChar(char Char, int LineNumber)
public static Dictionary<int, Dictionary<int, string>> TextEdit_QueryWord(string Word)
public static Dictionary<int, string> TextEdit_QueryWord(string Word, int LineNumber)
public static Dictionary<int, Dictionary<int, string>> TextEdit_QueryWordRegex(string Word)
public static Dictionary<int, string> TextEdit_QueryWordRegex(string Word, int LineNumber)
```
{% endcode %}

The query API for the text editor has been simplified as a result of having simpler ways of expressing the results. This was also done, because we needed to highlight the results found using one of the query commands in the text editor.

{% hint style="info" %}
The signatures have been changed. You can refer to the API documentation for the updated information.
{% endhint %}

### Aliases reworked

{% code title="AliasInfo.cs" lineNumbers="true" %}
```csharp
public static void PurgeAliases()
public static bool DoesAliasExistLocal(string TargetAlias, ShellType Type)
public static bool DoesAliasExistLocal(string TargetAlias, string Type)
```
{% endcode %}

The aliases system was a mess since a long ago, because it was first implemented using a single CSV file containing information about aliased commands.

We realized how this was going to affect its maintainability, so we decided to refresh it by switching to a JSON-based serialization and deserialization method, eliminating the 3 above functions.

{% hint style="info" %}
With regards to `PurgeAliases()`, once any alias is removed, they're automatically purged.

With regards to `DoesAliasExistLocal()`, you can substitute calls to that function with the `DoesAliasExist()`, making changes as necessary.
{% endhint %}

### Added the signing key (a.k.a. strong name)

Nitrocid KS launched without any signing key. Originally, it was planned to come signed by us, but it didn't work. However, we used the strong naming tool to give Nitrocid KS a unique identity to avoid dependency hell.

{% hint style="info" %}
It's not necessary to strong name your mods, but we recommend you to do so using the strong naming utility (`sn.exe`) that comes with Visual Studio.

Most of the time, you don't have to do anything to your mods, since the signing key change doesn't break any API functions. If you found that your mods no longer worked, try to update to the latest version of Nitrocid, or contact the mod vendor.
{% endhint %}

### Interactive TUI is now part of `ConsoleBase`

{% code title="*InteractiveTui*.cs" lineNumbers="true" %}
```csharp
namespace KS.Misc.Interactive
```
{% endcode %}

The interactive TUI started off as part of the `Misc` namespace found under `KS`. However, it became almost stable and ready for the prime time, so we decided to move it to `ConsoleBase` in preparation for the upcoming release of Terminaux, which brings performance-related improvements.

{% hint style="info" %}
Because this is a namespace move, you'll have to update the imports that point to the above namespace to point to the new namespace below:

```csharp
namespace KS.ConsoleBase.Interactive
```
{% endhint %}

### Used Figletize

{% code title="CenteredFigletTextColor.cs" lineNumbers="true" %}
```csharp
public static void WriteCenteredFiglet(int top, FiggleFont FigletFont, string Text, params object[] Vars)
public static void WriteCenteredFiglet(int top, FiggleFont FigletFont, string Text, KernelColorType ColTypes, params object[] Vars)
public static void WriteCenteredFiglet(int top, FiggleFont FigletFont, string Text, KernelColorType colorTypeForeground, KernelColorType colorTypeBackground, params object[] Vars)
public static void WriteCenteredFiglet(int top, FiggleFont FigletFont, string Text, ConsoleColors Color, params object[] Vars)
public static void WriteCenteredFiglet(int top, FiggleFont FigletFont, string Text, ConsoleColors ForegroundColor, ConsoleColors BackgroundColor, params object[] Vars)
public static void WriteCenteredFiglet(int top, FiggleFont FigletFont, string Text, Color Color, params object[] Vars)
public static void WriteCenteredFiglet(int top, FiggleFont FigletFont, string Text, Color ForegroundColor, Color BackgroundColor, params object[] Vars)
public static void WriteCenteredFiglet(FiggleFont FigletFont, string Text, params object[] Vars)
public static void WriteCenteredFiglet(FiggleFont FigletFont, string Text, KernelColorType ColTypes, params object[] Vars)
public static void WriteCenteredFiglet(FiggleFont FigletFont, string Text, KernelColorType colorTypeForeground, KernelColorType colorTypeBackground, params object[] Vars)
public static void WriteCenteredFiglet(FiggleFont FigletFont, string Text, ConsoleColors Color, params object[] Vars)
public static void WriteCenteredFiglet(FiggleFont FigletFont, string Text, ConsoleColors ForegroundColor, ConsoleColors BackgroundColor, params object[] Vars)
public static void WriteCenteredFiglet(FiggleFont FigletFont, string Text, Color Color, params object[] Vars)
public static void WriteCenteredFiglet(FiggleFont FigletFont, string Text, Color ForegroundColor, Color BackgroundColor, params object[] Vars)
```
{% endcode %}

This change also applies to `FigletColor` and `FigletWhereColor`.

Figgle didn't see any recent development for a long time, so we decided to tweak it a bit in our own fork, Figletize, prior to releasing it as version 0.6.0.

We decided to take out legacy support in both Nitrocid KS and Terminaux as Figgle wasn't signed using a signing key.

{% hint style="info" %}
You must migrate from Figgle to Figletize in order to be able to use the new functions. The signatures are the same, but using the `FigletizeFont` instance instead of `FiggleFont`.

Terminaux's documentation will cover the ways of migration.
{% endhint %}

### Added `CommandArgumentPart`

```csharp
public CommandArgumentInfo(string[] Arguments, SwitchInfo[] Switches)
public CommandArgumentInfo(string[] Arguments, SwitchInfo[] Switches, bool ArgumentsRequired, int MinimumArguments, bool AcceptsSet = false, Func<string, int, char[], string[]> AutoCompleter = null)
```

`CommandArgumentInfo` used to use strings and few support variables for arguments. However, there are cases where mods need more control over the argument parts, especially the auto completion part.

`CommandArgumentPart` is made for this purpose, so we've changed the above constructors in `CommandArgumentInfo` to be able to specify arguments using the brand new class.

{% hint style="info" %}
You must change all `CommandArgumentInfo` instance creation to satisfy this new format. For example, `exec`'s `CommandArgumentInfo` is defined like this:

```csharp
new CommandArgumentInfo(new[]
{
    new CommandArgumentPart(true, "process"),
    new CommandArgumentPart(false, "args")
}, Array.Empty<SwitchInfo>())
```
{% endhint %}

### Remote debug configuration handler changes

{% code title="IRemoteDebugCommand.cs" lineNumbers="true" %}
```csharp
void Execute(string StringArgs, string[] ListArgsOnly, string[] ListSwitchesOnly, string Address);
```
{% endcode %}

{% code title="RemoteDebugTools.cs" lineNumbers="true" %}
```csharp
// Removed
public enum DeviceProperty
public static object GetDeviceProperty(string DeviceIP, DeviceProperty DeviceProperty)
public static void SetDeviceProperty(string DeviceIP, DeviceProperty DeviceProperty, object Value)
public static bool TrySetDeviceProperty(string DeviceIP, DeviceProperty DeviceProperty, object Value)
public static void AddDeviceToJson(string DeviceIP, bool ThrowException = true)
public static bool TryAddDeviceToJson(string DeviceIP, bool ThrowException = true)
public static void RemoveDeviceFromJson(string DeviceIP)
public static bool TryRemoveDeviceFromJson(string DeviceIP)

// Modified
public static void DisconnectDbgDev(string IPAddr)
public static void AddToBlockList(string IP)
public static string[] ListDevices()
```
{% endcode %}

The remote debug device handler has undergone significant changes related to how it handles remote device configuration, because we needed a less convoluted way of obtaining device properties and setting them, so we decided to make the following changes as stated in the comments of the above snippet.

{% hint style="info" %}
As a result, custom remote debug command classes must now change the signature to match the below signature definition:

```csharp
void Execute(string StringArgs, string[] ListArgsOnly, string[] ListSwitchesOnly, RemoteDebugDeviceInfo Address);
```
{% endhint %}

### Speed dial configuration changes

{% code title="NetworkConnectionTools.cs" lineNumbers="true" %}
```csharp
public static void OpenConnectionForShell(ShellType shellType, Func<string, NetworkConnection> establisher, Func<string, JToken, NetworkConnection> speedEstablisher, string address = "")
public static void OpenConnectionForShell(string shellType, Func<string, NetworkConnection> establisher, Func<string, JToken, NetworkConnection> speedEstablisher, string address = "")
```
{% endcode %}

{% code title="SpeedDialTools.cs" lineNumbers="true" %}
```csharp
// Removed
public static JObject GetTokenFromSpeedDial()
public static JToken GetQuickConnectInfo()

// Modified
public static Dictionary<string, JToken> ListSpeedDialEntries()
public static Dictionary<string, JToken> ListSpeedDialEntriesByType(NetworkConnectionType SpeedDialType)
public static Dictionary<string, JToken> ListSpeedDialEntriesByType(string SpeedDialType)
```
{% endcode %}

The speed dial handler has undergone significant changes related to how it handles speed dial configuration, because we needed a less convoluted way of obtaining speed dial properties and setting them, so we decided to make the following changes as stated in the comments of the above snippet.

{% hint style="info" %}
Because `SpeedDialEntry` is implemented, you need to use the new overloads and functions from the SpeedDialTools class when migrating from the old `JToken` approach.
{% endhint %}

### Breaking changes caused by migration to addons

There are breaking changes that are caused by the migration of some of the optional kernel features to the addons system so that they don't bloat the base kernel up. The following applications are affected:

* To-do list
* Forecast
* Contacts
* Calendar
* Archive Shell
* Games and Amusements
* Timers
* Dictionary
* Name generation
* Unit conversion
* Lyrics
* Calculators
* Theme and Language studios
* Color conversion commands
* Time information commands

{% hint style="warning" %}
Your mods will no longer be able to use their APIs, due to how these addons are isolated from the base kernel.
{% endhint %}

### Removed Figgle-related tools and writers

{% code title="Deleted classes" lineNumbers="true" %}
```csharp
public static class CenteredFigletTextColorLegacy
public static class FigletColorLegacy
public static class FigletWhereColorLegacy
```
{% endcode %}

Because Terminaux got updated in a way that it separated Figgle-related tools and writers from the Figletize-based implementation, we've decided to remove all Figgle-related tools and writers from the Nitrocid KS codebase.

{% hint style="info" %}
You must migrate from Figgle to Figletize in order to be able to use the new functions. The signatures are the same, but using the `FigletizeFont` instance instead of `FiggleFont`.

Terminaux's documentation will cover the ways of migration.
{% endhint %}

### Moved `ShellManager` to `ShellBase.Shells`

```csharp
public static class ShellManager
```

`ShellManager` used to be found on a loose `KS.Shell` namespace. However, we needed to enforce better organization of the shell management code, so we moved this class to the `ShellBase.Shells` namespace inside `KS.Shell`.

{% hint style="info" %}
None of the functions are affected by this change. However, you need to change the imports to point to the `ShellBase.Shells` namespace.
{% endhint %}

### Moved `Editors` to `Files`

{% code title="Affected editors" lineNumbers="true" %}
```csharp
namespace KS.Misc.Editors.HexEdit
namespace KS.Misc.Editors.JsonShell
namespace KS.Misc.Editors.SqlEdit
namespace KS.Misc.Editors.TextEdit
```
{% endcode %}

The following editors have been moved from `Misc` to `Files` as a result of their APIs being mature:

* Hex editor
* JSON shell
* SQL editor
* Text editor

This is to acknowledge that these editors are indeed part of the filesystem editing process.

{% hint style="info" %}
None of the functions are affected, but you must change the namespace imports to the following:

```csharp
namespace KS.Files.Editors.HexEdit
namespace KS.Files.Editors.JsonShell
namespace KS.Files.Editors.SqlEdit
namespace KS.Files.Editors.TextEdit
```
{% endhint %}

### Updated `LocaleGen.Core` namespaces

```csharp
namespace Nitrocid.LocaleGen.Serializer
```

`LocaleGen.Core` is a library that generates final translation files for each built-in and custom language for Nitrocid KS to be able to use.

To clear up the organization, we've decided to update the above namespace to match the library name.

{% hint style="info" %}
The new namespace for the `LocaleGen.Core` functions is:

```csharp
namespace Nitrocid.LocaleGen.Core.Serializer
```
{% endhint %}

### Promoted Settings to `Kernel.Configuration`

{% code title="SettingsApp.cs, SettingsKeyType.cs" lineNumbers="true" %}
```csharp
namespace KS.Misc.Settings
```
{% endcode %}

Interactive settings was initially implemented as a Windows Forms application independent of the main application. It was moved to Nitrocid starting from 0.0.12.0.

The "Settings" application was meant to be a core part of the kernel, so we've moved all the Settings classes to the `KS.Kernel.Configuration.Settings` namespace.

{% hint style="info" %}
None of the classes are affected, but you should update your namespace imports to the below namespace:

```csharp
namespace KS.Kernel.Configuration.Settings
```
{% endhint %}

### Corrected the name of `Journaling` enum entry

{% code title="KernelPathType.cs" lineNumbers="true" %}
```csharp
public enum KernelPathType
{
    (...)
    Journalling,
    (...)
}
```
{% endcode %}

{% code title="Paths.cs" lineNumbers="true" %}
```csharp
public static string JournallingPath =>
    Filesystem.NeutralizePath(AppDataPath + "/KSJournal.json");
```
{% endcode %}

When kernel journaling was first released, it was initially implemented under the name of "Journalling," but we found out that it wasn't a correct spelling for the word.

We've decided to correct that by renaming the enumeration value and the property.

{% hint style="info" %}
If you use any of the two affected objects mentioned above, change your references so that it says `Journaling`.
{% endhint %}

### Moved presentation system to `ConsoleBase`

{% code title="Affected namespaces" lineNumbers="true" %}
```csharp
namespace KS.Misc.Presentation
namespace KS.Misc.Presentation.Elements
```
{% endcode %}

The presentation system used to be found in the Misc section of the kernel. However, it has been moved to a completely different section as it has to do more with printing things to the console.

{% hint style="info" %}
None of the classes are affected, but you should update your namespace imports to the below namespace:

```csharp
namespace KS.ConsoleBase.Presentation
namespace KS.ConsoleBase.Presentation.Elements
```
{% endhint %}

### Renamed `Flags` to `KernelFlags`

{% code title="Flags.cs" lineNumbers="true" %}
```csharp
namespace KS.Kernel
    public static class Flags
```
{% endcode %}

In order to maintain consistency for the naming scheme that we use for our base classes, we've renamed `Flags` to `KernelFlags`.

{% hint style="info" %}
None of the flags are affected. However, the following changes are made:

* Changed the class name to `KernelFlags`
* Changed the namespace to `KS.Kernel.Configuration`
{% endhint %}

### Moved argument and switch management

{% code lineNumbers="true" %}
```csharp
// Migrated to KS.Shell.ShellBase.Switches
public class SwitchInfo
public static class SwitchManager

// Migrated to KS.Shell.ShellBase.Commands
public static class ProcessExecutor

// Migrated to KS.Shell.ShellBase.Arguments
public static class ArgumentsParser
public class CommandArgumentInfo
public class CommandArgumentPart
public static class CommandAutoCompletionList
public class ProvidedArgumentsInfo
```
{% endcode %}

The `KS.Shell.ShellBase.Commands` namespace kept getting bigger with each new feature UESH receives from time to time, so we've decided to clear the confusion and move the above classes to their respective namespaces:

* `KS.Shell.ShellBase.Switches`
* `KS.Shell.ShellBase.Commands`
* `KS.Shell.ShellBase.Arguments`

{% hint style="info" %}
None of the functions are affected, but you must adjust the namespace imports to the above namespaces.
{% endhint %}

### `HelpDefinition`'s setter is private

{% code title="SwitchInfo.cs" lineNumbers="true" %}
```csharp
public string HelpDefinition { get; set; }
```
{% endcode %}

When `HelpDefinition` was first implemented, its setter was open, under the assumption that mods can adjust their help definitions when they define their commands.

However, with the improved shell and its command handling code, especially the inception of `CommandArgumentInfo` and its siblings, we've decided to finally make the setter private.

{% hint style="info" %}
You can no longer use the HelpDefinition property to directly set your command description. Instead, define your command description when making your own `CommandInfo` and its associated `CommandArgumentInfo` and `SwitchInfo` instances, if any.
{% endhint %}

### Implemented original argument string and list

{% code title="BaseCommand.cs" lineNumbers="true" %}
```csharp
public override int Execute(string StringArgs, string[] ListArgsOnly, string[] ListSwitchesOnly, ref string variableValue)
public override int ExecuteDumb(string StringArgs, string[] ListArgsOnly, string[] ListSwitchesOnly, ref string variableValue)
```
{% endcode %}

You only had a text of arguments and its list version, which were processed. However, we've considered cases where the unprocessed version might be more appropriate.

So, we've decided to implement the first two variables again, but, this time, the unprocessed version. That means no removal of any content in the text.

{% hint style="info" %}
The signature for these two functions has changed. You must change your override to match the new signature, like below:

```csharp
public override int Execute(string StringArgs, string[] ListArgsOnly, string StringArgsOrig, string[] ListArgsOnlyOrig, string[] ListSwitchesOnly, ref string variableValue)
public override int ExecuteDumb(string StringArgs, string[] ListArgsOnly, string StringArgsOrig, string[] ListArgsOnlyOrig, string[] ListSwitchesOnly, ref string variableValue)
```
{% endhint %}

### Moved parsers to the `Text` namespace

{% code title="Affected namespaces" lineNumbers="true" %}
```csharp
namespace KS.Misc.Probers.Motd
namespace KS.Misc.Probers.Placeholder
namespace KS.Misc.Probers.Regexp
```
{% endcode %}

These probers used to be loosely located on the `Probers` section of the `Misc` namespace. In order to improve organization, we've relocated these namespaces to the `Text` section of the `Probers` namespace.

{% hint style="info" %}
These namespaces have been located to the new location. They can be respectively found on:

```csharp
namespace KS.Misc.Text.Probers.Motd
namespace KS.Misc.Text.Probers.Placeholder
namespace KS.Misc.Text.Probers.Regexp
```
{% endhint %}

### Remove leftover namespace, `UserProperty`

{% code title="UserManagement.cs" lineNumbers="true" %}
```csharp
public enum UserProperty
```
{% endcode %}

`UserProperty` was first implemented on 0.0.16.0 as a result of converting the users configuration file to the JSON format from the old CSV format known to have flaws.

Over time, it was populated with the following properties:

* Username
* Password
* Admin
* Anonymous
* Disabled
* Permissions
* FullName
* PreferredLanguage
* Groups

However, we've made a third refinement to the login configuration logic, this time, using the JSON serialization and deserialization techniques to reduce the complexity.

As a result, `UserProperty` was rendered useless.

{% hint style="danger" %}
We advice you to cease using this enumeration. It's neccessary that you use more appropriate ways to get and set user properties to ensure that the mod code transition from Beta 2 or lower to Beta 3 goes smoothly.
{% endhint %}

### Moved argument tools

```csharp
namespace KS.Arguments.ArgumentBase
```

We've moved the argument tools from `ArgumentBase` to reduce the complexity. Now, the argument tools are no longer one namespace level deep.

{% hint style="info" %}
The argument tools are moved to the new namespace:

```csharp
namespace KS.Arguments
```
{% endhint %}

### Removed `UserProperty`

{% code title="UserManagement.cs" lineNumbers="true" %}
```csharp
public enum UserProperty
```
{% endcode %}

`UserProperty` was first implemented as individual flags, into which were turned into this enum somewhere during the development of the most anticipated version, 0.0.16.0. However, it has become increasingly complex to the point that we've made major changes to the configuration reader and writer in two stages during the development of 0.1.0.

As a result, this enum was rendered useless.

{% hint style="danger" %}
We advice you to cease using this enumeration. Make appropriate changes with the functions that `UserManagement` provides.
{% endhint %}

### Remote debugger provides `WriteDebugDeviceOnly()`

{% code title="IRemoteDebugCommand.cs" lineNumbers="true" %}
```csharp
void Execute(string StringArgs, string[] ListArgsOnly, string[] ListSwitchesOnly, RemoteDebugDeviceInfo Address);
```
{% endcode %}

Because we've implemented `RemoteDebugDeviceInfo` and `RemoteDebugDevice` to simplify the process of getting a device information from the list of remote debug devices, we've provided you with a handy function that allows you to write to one debug device, `WriteDebugDeviceOnly()`.

In consequence, we've had to change the Address parameter from `RemoteDebugDeviceInfo` to `RemoteDebugDevice` to allow easier access to this added function.

{% hint style="info" %}
The `Execute()` function has been changed, so you have to change the signature so that it becomes:

```csharp
void Execute(string StringArgs, string[] ListArgsOnly, string[] ListSwitchesOnly, RemoteDebugDevice device);
```
{% endhint %}

### `PromptForSetLang()` removed

{% code title="LanguageManager.cs" lineNumbers="true" %}
```csharp
public static void PromptForSetLang(string lang, bool Force = false, bool AlwaysTransliterated = false, bool AlwaysTranslated = false)
```
{% endcode %}

In the language manager, `PromptForSetLang()` was used for prompting the user to set the language. However, it wasn't used since `ChLang` was removed several milestones back. `ChLang` was brought back much later, but without the usage of this function. As a result, we've removed this function.

{% hint style="danger" %}
It's advisable to cease using this function.
{% endhint %}

### Moved custom screensaver tools

{% code title="CustomSaverTools.cs" lineNumbers="true" %}
```csharp
public static class CustomSaverTools
```
{% endcode %}

The customized screensaver tools have been moved to the screensaver management class, because we saw that there was absolutely no need for a specialized class for such tools. This allows easier access to the registration and unregistration functions.

{% hint style="info" %}
You must change your reference to all the tools found inside the above class to the `ScreensaverManager` class if you're using one of these:

* `RegisterCustomScreensaver()`
* `UnregisterCustomScreensaver()`
{% endhint %}

### `AutoCompleter` only requires an array

{% code title="CommandAutoCompletionList.cs" lineNumbers="true" %}
```csharp
public static Func<string, int, char[], string[]> GetCompletionFunction(string expression)
```
{% endcode %}

{% code title="CommandArgumentPart.cs" lineNumbers="true" %}
```csharp
public Func<string, int, char[], string[]> AutoCompleter { get; private set; }
public CommandArgumentPart(bool argumentRequired, string argumentExpression, Func<string, int, char[], string[]> autoCompleter = null)
```
{% endcode %}

Previously, when we used the deprecated ReadLine.Reboot library to handle autocompletion, we used to pass in the three parameters. Now that Nitrocid KS's auto completion handler automatically chops the string to only return the completion of the argument (file name, user name, etc.), these three parameters are no longer needed.

{% hint style="info" %}
If you're not using these three parameters, the transition is easy. However, if you are using one of them, you'll need to refactor your command info code so that it matches the requirements.
{% endhint %}

### Added `CommandParameters`

{% code title="ICommand.cs" lineNumbers="true" %}
```csharp
int Execute(string StringArgs, string[] ListArgsOnly, string StringArgsOrig, string[] ListArgsOnlyOrig, string[] ListSwitchesOnly, ref string variableValue);
int ExecuteDumb(string StringArgs, string[] ListArgsOnly, string StringArgsOrig, string[] ListArgsOnlyOrig, string[] ListSwitchesOnly, ref string variableValue);
```
{% endcode %}

The base command's Execute function used to only host two parameters, which were the first two arguments in the above function. Since then, many things were added, like original argument string and list, variable setting, and list of switches.

This caused us to introduce the CommandParameters class to allow us to introduce further changes (further additions or improvements) without breaking the whole signature of the above function.

{% hint style="info" %}
You must change all your command classes to have the below signatures:

```csharp
int Execute(CommandParameters parameters, ref string variableValue);
int ExecuteDumb(CommandParameters parameters, ref string variableValue);
```
{% endhint %}

### Customizable settings entries

This breaking change is divided to the below parts, from the oldest to the newest:

#### Initial migration of settings entries

{% code title="ConfigTools.cs" lineNumbers="true" %}
```csharp
public static object GetValueFromEntry(JToken Setting, ConfigType SettingsType)
public static List<InputChoiceInfo> FindSetting(string Pattern, JToken SettingsToken, ConfigType SettingsType)
```
{% endcode %}

{% code title="SettingsApp.cs" lineNumbers="true" %}
```csharp
public static void OpenSection(string Section, JToken SettingsToken, ConfigType SettingsType)
public static void OpenKey(string Section, int KeyNumber, JToken SettingsToken, ConfigType SettingsType)
public static void VariableFinder(JToken SettingsToken, ConfigType SettingsType)
private static JToken OpenSettingsResource(ConfigType SettingsType)
```
{% endcode %}

Currently, the maintenance of the settings application is complex, because the settings application contained three `Open*` functions that basically did everything.

We've decided to take the first steps by converting the settings entry list embedded JSON files to arrays of classes containing the entry information. Basically, it created two classes dedicated to that:

* `SettingsEntry`: Represents a section
* `SettingsKey`: Represents a settings key with its options

{% hint style="warning" %}
Currently, it's not possible to use the above functions anymore with the built-in Nitrocid settings entries, because it relies on a private function that gives you the necessary infromation.
{% endhint %}

#### Initial support for custom settings

{% code title="ConfigTools.cs" lineNumbers="true" %}
```csharp
public static object GetValueFromEntry(SettingsKey Setting, ConfigType SettingsType)
public static List<InputChoiceInfo> FindSetting(string Pattern, SettingsEntry[] SettingsEntries, ConfigType SettingsType)
```
{% endcode %}

{% code title="SettingsApp.cs" lineNumbers="true" %}
```csharp
public static void OpenSection(string Section, SettingsEntry SettingsSection, ConfigType SettingsType)
public static void OpenKey(int KeyNumber, SettingsEntry SettingsSection, ConfigType SettingsType)
public static void VariableFinder(SettingsEntry[] SettingsEntries, ConfigType SettingsType)
```
{% endcode %}

Custom settings are implemented to increase the flexibility of the kernel configuration so that your mods can now have their own settings entry list file. This means that your mods are now configurable in a way that Nitrocid's settings application can handle all your settings.

#### Finalization of the custom settings

<pre class="language-csharp" data-title="KernelPathType.cs" data-line-numbers><code class="lang-csharp">public enum KernelPathType
{
    (...)
<strong>    /// &#x3C;summary>
</strong><strong>    /// Splash configuration file.
</strong><strong>    /// &#x3C;/summary>
</strong><strong>    SplashConfiguration,
</strong>    (...)
}
</code></pre>

{% code title="Config.cs" lineNumbers="true" %}
```csharp
public static KernelSplashConfig SplashConfig
```
{% endcode %}

<pre class="language-csharp" data-title="ConfigType.cs" data-line-numbers><code class="lang-csharp">public enum ConfigType
{
<strong>    /// &#x3C;summary>
</strong><strong>    /// Splash configuration
</strong><strong>    /// &#x3C;/summary>
</strong><strong>    Splash
</strong>}
</code></pre>

The custom settings feature has been finalized to the point that it's reached the stable stage, so we've decided to move almost all the screensaver and splash entries (from their addons) from the base Nitrocid configuration entry JSON files to their appropriate entries.

#### Removed the `ConfigType` enumeration

{% code title="Config.cs" lineNumbers="true" %}
```csharp
public static BaseKernelConfig GetKernelConfig(ConfigType type)
public static void CreateConfig(ConfigType type, string ConfigPath)
public static bool TryCreateConfig(ConfigType type, string ConfigPath)
public static void ReadConfig(ConfigType type)
public static void ReadConfig(ConfigType type, string ConfigPath)
public static bool TryReadConfig(ConfigType type)
public static bool TryReadConfig(ConfigType type, string ConfigPath)
```
{% endcode %}

{% code title="ConfigTools.cs" lineNumbers="true" %}
```csharp
public static SettingsEntry[] OpenSettingsResource(ConfigType SettingsType)
public static string TranslateBuiltinConfigType(ConfigType SettingsType)
```
{% endcode %}

{% code title="ConfigType.cs" lineNumbers="true" %}
```csharp
public enum ConfigType
```
{% endcode %}

{% code title="SettingsApp.cs" lineNumbers="true" %}
```csharp
public static void OpenMainPage(ConfigType SettingsType)
```
{% endcode %}

This is the final part of the configuration system customizability. This change removes the `ConfigType` enumeration to increase consistency across the custom settings functionality, thus removing several of the above functions.

{% hint style="info" %}
This configuration system overhaul is massive, so we recommend that you:

* Stop using the removed functions,
* Adjust your code to align with the new configuration system, and
* Migrate your "custom settings app" for your mods (if implemented) to use Nitrocid's settings handler by registering and unregistering your settings classes as per guidance.
{% endhint %}

### Help system refactors

{% code title="HelpSystem.cs" lineNumbers="true" %}
```csharp
public static class HelpSystem
```
{% endcode %}

The help system code has been moved to its own namespace, because it has nothing to do with command management or execution, although it displays the usage of the commands. This is to ensure consistent organization of the UESH shell portion of the simulated system, backed by the Nitrocid kernel.

{% hint style="info" %}
This class has been moved to the `HelpPrint` class in the `KS.Shell.ShellBase.Help` namespace.
{% endhint %}

### Moved several filesystem classes

```csharp
namespace KS.Files.Print
namespace KS.Files.Querying
namespace KS.Files.Read
```

Several of the filesystem classes were moved to more appropriate places so that we can ensure better organization related to the filesystem namespace.

{% hint style="info" %}
All classes which were located on the Print namespace were mvoed to `KS.Files.Operations.Printing`.

Similarly, all querying classes, such as `Checking` and `Parsing`, were moved to `KS.Files.Operations.Querying`.

Also, the reading class name was changed to `Reading` and its namespace changed to `KS.Files.Operations`.

Additionally, the `ReadToEndAndSeek()` function was moved to the Reading class, causing the `StreamRead` class to be removed.
{% endhint %}

### Conflicting write overloads reduced

{% code title="TextWriterColor.cs and all similar writers" lineNumbers="true" %}
```csharp
public static void Write(string Text, bool Line, bool Highlight, KernelColorType colorType, params object[] vars)
public static void Write(string Text, bool Line, KernelColorType colorTypeForeground, KernelColorType colorTypeBackground, params object[] vars)
public static void Write(string Text, bool Line, bool Highlight, KernelColorType colorTypeForeground, KernelColorType colorTypeBackground, params object[] vars)
public static void Write(string Text, bool Line, ConsoleColors color, params object[] vars)
public static void Write(string Text, bool Line, bool Highlight, ConsoleColors color, params object[] vars)
public static void Write(string Text, bool Line, ConsoleColors ForegroundColor, ConsoleColors BackgroundColor, params object[] vars)
public static void Write(string Text, bool Line, bool Highlight, ConsoleColors ForegroundColor, ConsoleColors BackgroundColor, params object[] vars)
public static void Write(string Text, bool Line, Color color, params object[] vars)
public static void Write(string Text, bool Line, bool Highlight, Color color, params object[] vars)
public static void Write(string Text, bool Line, Color ForegroundColor, Color BackgroundColor, params object[] vars)
public static void Write(string Text, bool Line, bool Highlight, Color ForegroundColor, Color BackgroundColor, params object[] vars)
```
{% endcode %}

Ever since Terminaux was released to the public, the `Color` class had an implicit operator which would take either an integer, a string, or a `ConsoleColors` value and create a new `Color` class based on these values. This caused us to have to fix every function call that contain strings as their first parameters, since they were mistaken for creating a new color, causing graphical artifacts.

We've recommended mod developers who suffer from this ambiguity issue append a `vars:` prefix in its appropriate place and write `new object[] { args }`, but this is a big overhead.

So, we've decided to rename function names that take colors to these variants:

* `Write()`: for plain writing in default colors
* `WriteColor()`: for writing with custom foreground colors
* `WriteColorBack()`: for writing with custom foreground and background colors
* `WriteKernelColor()`: for writing with kernel-defined color types

{% hint style="info" %}
You no longer need to override the `vars` value using the above method. Instead, you can replace these calls with one of the above functions, based on the color type.
{% endhint %}

### Moved hardware probe tools to the driver

{% code title="Config and Flags" lineNumbers="true" %}
```csharp
// KernelMainConfig.cs
public bool FullHardwareProbe { get; set; }

// KernelFlags.cs
public static bool FullHardwareProbe
```
{% endcode %}

{% code title="Probe tools" lineNumbers="true" %}
```csharp
public static void ListHardware(string HardwareType)
// where HardwareType is a type other than the supported types by the hardware prober driver
```
{% endcode %}

The hardware prober used to be a wrapper for a single hardware specification library, Inxi.NET, that probed everything, including the RAM, the CPU, and the GPU. Over time, Inxi.NET faced lots of JSON management problems, including a recent change to the JSON output back on a version that was released between late 2020 and April 2021.

As a result, we're slowly migrating to the completely new hardware probing library that will be hopefully faster than Inxi.NET.

Unfortunately, full hardware probing has been removed.

{% hint style="info" %}
`ListHardware`'s behavior has changed so that it only gets information about supported hardware types.
{% endhint %}

### New `ThemeInfo` by theme name removed

{% code title="ThemeInfo.cs" lineNumbers="true" %}
```csharp
[Obsolete("Theme addons can't use this. It's only useful for getting the default theme.")]
public ThemeInfo(string ThemeResourceName)
```
{% endcode %}

Ever since themes were moved to their own separate place, called Theme Packs, we've decided to remove this obsolete constructor, because it was rendered useless and can only search for the default theme.

Additionally, it can't handle addon themes, because Nitrocid isn't supposed to access other addon's resources, so we've decided to make a final decision to remove the above constructior.

{% hint style="info" %}
We advice you not to use this constructor to create new `ThemeInfo` instances.
{% endhint %}

### Improved support for multiple `ArgInfos`

{% code title="ArgumentsParser.cs" lineNumbers="true" %}
```csharp
public static ProvidedArgumentsInfo ParseShellCommandArguments(string CommandText, ShellType CommandType)
public static ProvidedArgumentsInfo ParseShellCommandArguments(string CommandText, string CommandType)
public static ProvidedArgumentsInfo ParseArgumentArguments(string ArgumentText)
```
{% endcode %}

Multiple argument info instances were supported in one of the Nitrocid KS 0.1.0 pre-release versions, including milestones, but they were not given enough testing for edge cases, such as calendar. As a result of the recent changes that were made to the shell, it no longer worked properly.

We've decided to improve support for multiple argument information instances by refactoring the code base in a way that makes UESH recognize this edge case.

{% hint style="info" %}
For the most part, you don't need to call these functions, unless you're explicitly using them for some reason; `CommandParameters parameters` is more than enough.
{% endhint %}

### Initial configuration for presets

{% code title="PromptPresetManager.cs" lineNumbers="true" %}
```csharp
public static void SetPresetDry(string PresetName, ShellType ShellType, bool ThrowOnNotFound = true)
public static void SetPresetDry(string PresetName, string ShellType, bool ThrowOnNotFound = true)
public static void PromptForPresets()
public static void PromptForPresets(ShellType shellType)
public static void PromptForPresets(string shellType)
```
{% endcode %}

We're in the process of changing how setting the presets up works in a way that it would deal with custom shell presets, such as those that were installed by the addons.

Earlier, the preset manager would only save changes for the built-in Nitrocid shells, but we wanted to extend this ability to save your preset selection to your custom shells.

{% hint style="info" %}
The `SPreset` settings has changed so that it would align with the set preset. `SetPresetDry()` has been removed, because we've made `SetPreset()` dry.
{% endhint %}

### KernelFlags removed

{% code title="KernelFlags.cs" lineNumbers="true" %}
```csharp
public class KernelFlags
```
{% endcode %}

The central KernelFlags class used to hold all the read-only flags that mods could access. Since it has shown a need for an unnecessary reference to a Configuration namespace of the kernel, we've decided to remove the KernelFlags class completely.

The flags themselves, however, are moved to their respective classes, depending on which feature uses the flag. Here are the three examples:

* `ConsoleSupportsTrueColor`: Moved to `ConsoleExtensions`
* `KernelShutdown`: Moved to `PowerManager`
* `QuietKernel`: Moved to `KernelEntry`

{% hint style="info" %}
The full list is being populated soon, due to current network-wide problems that prevent us from filling this list.

Once the full list is populated, follow this list to be able to migrate from `KernelFlags`.
{% endhint %}

### Changes to the time zone API

{% code title="TimeZones.cs" lineNumbers="true" %}
```csharp
public static Dictionary<string, DateTime> GetTimeZones()
```
{% endcode %}

We've made changes to the time zone API so that we could add new features to the API. However, this API had an outstanding problem with GetTimeZones() that did one thing too much: getting the current date and time with the time zone.

So, we've decided to split this function to two functions:

* `GetTimeZoneNames()`: Gets the time zone names
* `GetTimeZoneTimes()`: Gets the time zone times using the current date and time

{% hint style="info" %}
We advice you to use one of these functions, depending on whether you want time zone names or current times.
{% endhint %}

### Renamed two classes

{% code title="Presentation.cs" lineNumbers="true" %}
```csharp
public class Presentation
```
{% endcode %}

{% code title="Filesystem.cs" lineNumbers="true" %}
```csharp
public static class Filesystem
```
{% endcode %}

The two classes have been reported to use the same name as the namespace, so we've decided to rename the two above classes to avoid ambiguity.

You no longer have to fully qualify these two classes anytime you need access to these classes.

{% hint style="info" %}
The below classes have been renamed:

* `Presentation` -> `Slideshow`
* `Filesystem` -> `FilesystemTools`
{% endhint %}

### Used separate settings entry for default figlet font

{% code title="WelcomeMessage.cs" lineNumbers="true" %}
```csharp
public static string BannerFigletFont
```
{% endcode %}

{% code title="KernelMainConfig.cs" lineNumbers="true" %}
```csharp
public string BannerFigletFont
```
{% endcode %}

BannerFigletFont was used by literally one feature, which is an old feature. However, we wanted to increase the customizability of this feature, so we've decided to implement a new settings entry for the default figlet font to replace the two above properties.

{% hint style="info" %}
This settings entry still represents the name, so you just need to change from `BannerFigletFont` to `TextTools.DefaultFigletFontName`
{% endhint %}

### Refactored the arguments help system

{% code title="ArgumentHelpSystem.cs" lineNumbers="true" %}
```csharp
public static class ArgumentHelpSystem
```
{% endcode %}

The arguments help system has been refactored exactly as we've done earlier to the UESH help system in an attempt to increase consistency and maintainability.

The printing tools, however, stay internal until Beta 3 gets released after enough tests have been made.

{% hint style="info" %}
None of the functions found inside the class have been affected. It's just that you have to update the class name to `ArgumentHelpPrint` and the imports to `KS.Arguments.Help`.
{% endhint %}

### Refactored the remote debug help system

{% code title="RemoteDebugHelpSystem.cs" lineNumbers="true" %}
```csharp
public static class RemoteDebugHelpSystem
```
{% endcode %}

The remote debug help system has been refactored exactly as we've done earlier to the UESH help system in an attempt to increase consistency and maintainability.

The printing tools, however, stay internal until Beta 3 gets released after enough tests have been made.

{% hint style="info" %}
None of the functions found inside the class have been affected. It's just that you have to update the class name to `RemoteDebugHelpPrint` and the imports to `KS.Kernel.Debugging.RemoteDebug.Command.Help`.
{% endhint %}

### No prefixing for editor functions

When text and file editors were implemented, we had to prefix every command and function with the editor type prefix, separated by the underscore character and the actual function name, such as `JsonShell_OpenJsonFile()`.

However, this was a requirement as we had been using Visual Basic as the langauge when we first developed the first version of Nitrocid KS and every component had a single namespace at the time, which is `KS` before the migration.

Since we've moved to using namespaces and to using C# as the language used for the development of 0.1.0, we've decided to remove the prefixing from the editor functions and all the shell commands to show that namespaces are useful.

{% hint style="info" %}
You can invoke the editor functions by removing the prefix, such as `JsonShell_OpenJsonFile()` being just `OpenJsonFile()` from `JsonTools`.
{% endhint %}

### Moved `ShellStart` and `ShellTypeManager` functions

{% code title="ShellStart.cs" lineNumbers="true" %}
```csharp
public static class ShellStart
public static class ShellTypeManager
```
{% endcode %}

We're aiming for simplicity and stability across the Nitrocid API, so we've moved all the functions found in the two above classes to ShellManager, making them easier to access.

{% hint style="info" %}
None of the functions or their signatures are changed. Update all the references to `ShellStart` and `ShellTypeManager` to `ShellManager`.
{% endhint %}

### Help helpers are now turned to methods in argument info

{% code title="ArgumentInfo.cs" lineNumbers="true" %}
```csharp
public Action AdditionalHelpAction { get; private set; }
public ArgumentInfo(string Argument, string HelpDefinition, CommandArgumentInfo[] ArgArgumentInfo, ArgumentExecutor ArgumentBase, bool Obsolete = false, Action AdditionalHelpAction = null)
```
{% endcode %}

Earlier, we used to have a property in `ArgumentInfo`, called `AdditionalHelpAction`, that contains a function telling you about tips for using a specified command when the help system is being requested to display the usage information to the user, similar to how it worked for the UESH shell.

Because we've implemented the `arghelp` command to the administrative shell, and we wanted to stay consistent in terms of performance, we've decided to replace the `AdditionalHelpAction` property with the `HelpHelper()` function that is overridable from the base argument executor class, `ArgumentExecutor`.

{% hint style="info" %}
If you want additional help to be displayed, move the additional help action code to the HelpHelper() block, assuming that you've overridden it like below:

```csharp
public override void HelpHelper() =>
    TextWriterColor.Write("Your additional help here");
```
{% endhint %}

### Implemented `NotificationProgressState`

{% code title="Notification.cs" lineNumbers="true" %}
```csharp
public bool ProgressFailed { get; set; }
```
{% endcode %}

`ProgressFailed` was implemented as a way to indicate that the progress has failed. Back then, the progress being 100% meant that the progress notification indicates that the progress is complete. However, we wanted a more accurate way to have the basic progress statistics.

As a result, we need to implement the `NotificationProgressState` enumeration to more accurately represent the progress state for a single progress notification, determinate or not.

{% hint style="info" %}
If you want the progress notification to indicate failure, you'll have to set the `ProgressState` property value to `NotificationProgressState.Failure`.
{% endhint %}

### Auto completer gains access to the list of last arguments

{% code title="CommandArgumentPart.cs" lineNumbers="true" %}
```csharp
public Func<string[]> AutoCompleter { get; private set; }
public CommandArgumentPart(bool argumentRequired, string argumentExpression, Func<string[]> autoCompleter = null, bool isNumeric = false, string exactWording = null)
```
{% endcode %}

{% code title="CommandArgumentPartOptions.cs" lineNumbers="true" %}
```csharp
public Func<string[]> AutoCompleter { get; set; }
```
{% endcode %}

The auto completer now has access to the list of last passed arguments. This means that the auto completer function can now adapt to the passed arguments, such as listing all extension handlers for the specific extension.

We had to change the auto completer part to `Func<string[], string[]>` in order to accommodate this change.

{% hint style="info" %}
Change the auto completer definitions in your `CommandArgumentPart` instances to hold the new function signature. This new function signature holds information about the last passed arguments.
{% endhint %}

### Process execution moved

{% code title="ProcessExecutor.cs" lineNumbers="true" %}
```csharp
public static class ProcessExecutor
```
{% endcode %}

The `ProcessExecutor` was located in the following namespace:

{% code title="ProcessExecutor.cs" lineNumbers="true" %}
```csharp
namespace KS.Shell.ShellBase.Commands
```
{% endcode %}

This namespace was normally reserved for built-in shell commands. However, the process execution has nothing to do with the built-in UESH commands, although the shell considers these as commands. They're just external processes that can be executed right from the shell.

As a result, we've moved the `ProcessExecutor` class to the `ProcessExecution` part of the above namespace.

{% hint style="info" %}
The `ProcessExecutor` class can now be called by importing the below namespace:

* `KS.Shell.ShellBase.Commands.ProcessExecution`
{% endhint %}

### Moved `KernelTools`' properties to `KernelMain`

{% code title="KernelTools.cs" lineNumbers="true" %}
```csharp
public static class KernelTools
```
{% endcode %}

The kernel entry point used to have the name of Kernel since the inception of Nitrocid KS (Kernel Simulator back then) on 2018. Over time, the kernel got improved to the point that it's now become a massive operating system simulator that contains many useful tools.

As a result, we've moved the `KernelVersion` and the `KernelApiVersion` properties to the `KernelMain` class, which hosts the entry point of the simulator, and removed the `KernelTools` class.

{% hint style="info" %}
You can still access the two properties, though they're now re-located to the `KernelMain` class in the `KS.Kernel` namespace.
{% endhint %}

### `Out` Property Removed

{% code title="ConsoleWrapper.cs" lineNumbers="true" %}
```csharp
public static TextWriter Out
```
{% endcode %}

This property was added as part of the console wrapper class and as part of the console driver. It was added back when the shell had to manipulate with this property to nullify any output coming from the cancellation handler.

The thing is that this property was made under the assumption that it can be used to write output to the console and that it can be set, despite it being only useful for low-level scenarios, which are extremely rare.

As a result, for safety, we've decided to remove this property. The next breaking change is going to be about opening the input, the output, and the error output stream.

{% hint style="danger" %}
We advice you to stop using this property.
{% endhint %}

### Moved `SpeedDial` to Network Base

{% code title="SpeedDial*.cs" lineNumbers="true" %}
```csharp
namespace KS.Network.SpeedDial
```
{% endcode %}

This speed dial namespace menioned above was moved to the network base namespace, `KS.Network.Base.SpeedDial`. The reason for this migration is that because the speed dial functionality is integrated with the base network features.

{% hint style="info" %}
None of the speed dial classes (or any of their associated functions) have been changed. It's just that you have to update your usings so that it points to the `KS.Network.Base.SpeedDial` namespace.
{% endhint %}

### Events and reminders config changes

When it comes to the events and the reminders feature in the Calendars addon, you may notice that their configuration file formats have changed from the XML representation of said events or reminders to the JSON representation.

As a result, you'll have to manually change your configuration files related to such events and reminders to follow this suit:

* File extension is renamed from `.ksevent` or `.ksreminder` to `.json`
* Contents changed from XML to JSON

{% hint style="info" %}
Until tests are done, there are no clear instructions as to how to convert these configuration files. Be patient as they'll come soon.
{% endhint %}

### Removed command type from `CommandInfo`

{% code title="CommandInfo.cs" lineNumbers="true" %}
```csharp
public string Type { get; private set; }
public CommandInfo(string Command, ShellType Type, string HelpDefinition, CommandArgumentInfo[] CommandArgumentInfo, BaseCommand CommandBase, CommandFlags Flags = CommandFlags.None)
public CommandInfo(string Command, string Type, string HelpDefinition, CommandArgumentInfo[] CommandArgumentInfo, BaseCommand CommandBase, CommandFlags Flags = CommandFlags.None)
```
{% endcode %}

The command type used to be required when defining `CommandInfo` instances back when the shell didn't have a straightforward way in defining shell commands across different shells. This was proven difficult when defining custom mod commands, essentially making the mod developers either use `Shell` as a workaround, which is essentially clumsy, or make a custom shell for just their commands, which is ugly.

So, amidst the recent changes made to the core of the UESH, we've decided that the shell type argument is redundant, especially when `RegisterCustomCommand()` and its siblings appeared. As a result of this function appearing, we've decided to finally nerf this requirement out of the definition of the `CommandInfo`, essentially making it easier to use.

This was planned a long time ago, but it didn't really occur to us that such a change required tons of core changes to the UESH shell handling facility to get rid of extraneous arguments that are required.

{% hint style="info" %}
Just remove all references to the shell type in the second argument in your `CommandInfo` definitions, and you're good to go. Then, use `RegisterCustomCommand()` or its siblings to register them to your custom shell or one of the Nitrocid shells, such as `Shell`.
{% endhint %}

### Removed `ListModsStartingWith()`

{% code title="ModManager.cs" lineNumbers="true" %}
```csharp
public static Dictionary<string, ModInfo> ListModsStartingWith(string SearchTerm)
```
{% endcode %}

We've removed this function, because it was made only to help the UESH auto completer find all your mods.

{% hint style="info" %}
If you really want this function, you can re-implement it using [this link](https://github.com/Aptivi/NitrocidKS/commit/a79faa58481e72a92cb46ca67bd1a6551853c909) as a reference.
{% endhint %}

### Placeholder management handles arguments

{% code title="PlaceParse.cs" lineNumbers="true" %}
```csharp
public static void RegisterCustomPlaceholder(string placeholder, Func<string> placeholderAction)
```
{% endcode %}

The placeholder management used to be rigid when it came to parsing advanced placeholders with arguments separated by the first colon. We wanted to extend this functionality to be able to register placeholders that are able to behave according to the argument passed to it, so we've decided to introduce a new class, `PlaceInfo`, and change the placeholder action to hold the argument as the first argument.

{% hint style="info" %}
You must change your code so that the placeholder actions pass the first argument, if necessary. Example is provided in the placeholders page.

```csharp
public static void RegisterCustomPlaceholder(string placeholder, Func<string, string> placeholderAction)
```
{% endhint %}

### Language information class overhauled

{% code title="CultureManager.cs" lineNumbers="true" %}
```csharp
public static List<CultureInfo> GetCulturesFromCurrentLang()
public static List<CultureInfo> GetCulturesFromLang(string Language)
```
{% endcode %}

{% code title="LanguageInfo.cs" lineNumbers="true" %}
```csharp
public readonly string ThreeLetterLanguageName;
public readonly string FullLanguageName;
public readonly int Codepage;
public readonly string CultureCode;
public readonly string Country;
public readonly bool Transliterable;
public readonly bool Custom;
public readonly Dictionary<string, string> Strings;
public readonly List<CultureInfo> Cultures;
```
{% endcode %}

While we were converting all the classes to use properties in the middle of the 0.1.0 development cycle, we came across LanguageInfo still using read-only fields, like Codepage and Transliterable, that we completely forgot to update.

We've recently converted these read-only fields to properties so that consistency was achieved. As a consequence, two functions from the language manager have to be changed.

{% hint style="info" %}
The return value for these functions have changed to `CultureInfo[]`, so you need to make necessary changes.
{% endhint %}

### Moved some shells to their own individual addons

{% code title="ShellType.cs" lineNumbers="true" %}
```csharp
public enum ShellType
{
    FTPShell,
    MailShell,
    HTTPShell,
    RSSShell,
    SFTPShell,
    JsonShell,
    SqlShell,
}
```
{% endcode %}

Some shells used to be defined directly by the base kernel for easy access. Since the introduction of addons, many features that didn't directly have to do with the core kernel got moved.

Now, in addition to these features, the following shells got moved:

* FTP Shell (networked)
* HTTP Shell (networked)
* Mail Shell (networked)
* RSS Shell (networked)
* SFTP Shell (networked)
* JSON Shell (non-networked)
* SQL Shell (non-networked)

This means that your mods can't directly access them, their settings, and their tools anymore.

{% hint style="info" %}
The base network is not going to be moved to the addons, but expect its namespace to be updated soon.
{% endhint %}

### Inter-mod communication updates

{% code title="ModManager.cs" lineNumbers="true" %}
```csharp
public static object ExecuteCustomModFunction(string modName, string functionName)
public static object ExecuteCustomModFunction(string modName, string functionName, params object[] parameters)
```
{% endcode %}

The inter-mod communication facility was expanded to include support for properties and fields. As a result, a separate class was needed to avoid bloating the kernel modification management class.

The creation of the `InterModTools` class is to organize all the inter-mod communication-related functions.

{% hint style="info" %}
To continue using `ExecuteCustomModFunction()`, change the reference from `ModManager` to `InterModTools`.
{% endhint %}

### Separated the input info box to its own class

{% code title="InfoBoxColor.cs" lineNumbers="true" %}
```csharp
public static string WriteInfoBoxPlainInput(string text, params object[] vars)
public static string WriteInfoBoxPlainInput(string text, char UpperLeftCornerChar, char LowerLeftCornerChar, char UpperRightCornerChar, char LowerRightCornerChar, char UpperFrameChar, char LowerFrameChar, char LeftFrameChar, char RightFrameChar, params object[] vars)
public static string WriteInfoBoxInput(string text, params object[] vars)
public static string WriteInfoBoxInputKernelColor(string text, KernelColorType InfoBoxColor, params object[] vars)
public static string WriteInfoBoxInputKernelColor(string text, KernelColorType InfoBoxColor, KernelColorType BackgroundColor, params object[] vars)
public static string WriteInfoBoxInputColor(string text, ConsoleColors InfoBoxColor, params object[] vars)
public static string WriteInfoBoxInputColorBack(string text, ConsoleColors InfoBoxColor, ConsoleColors BackgroundColor, params object[] vars)
public static string WriteInfoBoxInputColor(string text, Color InfoBoxColor, params object[] vars)
public static string WriteInfoBoxInputColorBack(string text, Color InfoBoxColor, Color BackgroundColor, params object[] vars)
public static string WriteInfoBoxInput(string text, char UpperLeftCornerChar, char LowerLeftCornerChar, char UpperRightCornerChar, char LowerRightCornerChar, char UpperFrameChar, char LowerFrameChar, char LeftFrameChar, char RightFrameChar, params object[] vars)
public static string WriteInfoBoxInputKernelColor(string text, char UpperLeftCornerChar, char LowerLeftCornerChar, char UpperRightCornerChar, char LowerRightCornerChar, char UpperFrameChar, char LowerFrameChar, char LeftFrameChar, char RightFrameChar, KernelColorType InfoBoxColor, params object[] vars)
public static string WriteInfoBoxInputKernelColor(string text, char UpperLeftCornerChar, char LowerLeftCornerChar, char UpperRightCornerChar, char LowerRightCornerChar, char UpperFrameChar, char LowerFrameChar, char LeftFrameChar, char RightFrameChar, KernelColorType InfoBoxColor, KernelColorType BackgroundColor, params object[] vars)
public static string WriteInfoBoxInputColor(string text, char UpperLeftCornerChar, char LowerLeftCornerChar, char UpperRightCornerChar, char LowerRightCornerChar, char UpperFrameChar, char LowerFrameChar, char LeftFrameChar, char RightFrameChar, Color InfoBoxColor, params object[] vars)
public static string WriteInfoBoxInputColorBack(string text, char UpperLeftCornerChar, char LowerLeftCornerChar, char UpperRightCornerChar, char LowerRightCornerChar, char UpperFrameChar, char LowerFrameChar, char LeftFrameChar, char RightFrameChar, Color InfoBoxColor, Color BackgroundColor, params object[] vars)
public static string WriteInfoBoxInputColor(string text, char UpperLeftCornerChar, char LowerLeftCornerChar, char UpperRightCornerChar, char LowerRightCornerChar, char UpperFrameChar, char LowerFrameChar, char LeftFrameChar, char RightFrameChar, ConsoleColors InfoBoxColor, params object[] vars)
public static string WriteInfoBoxInputColorBack(string text, char UpperLeftCornerChar, char LowerLeftCornerChar, char UpperRightCornerChar, char LowerRightCornerChar, char UpperFrameChar, char LowerFrameChar, char LeftFrameChar, char RightFrameChar, ConsoleColors InfoBoxColor, ConsoleColors BackgroundColor, params object[] vars)
```
{% endcode %}

The input version of the informational box used to reside on the same class as the normal modal and non-modal informational boxes. However, over time, we've discovered that we didn't need to bloat the class with input-related infoboxes. We've moved all the above functions to their own separate class, `InfoBoxInputColor`.

{% hint style="info" %}
None of the functions were altered during the move. You must change the reference to `InfoBoxColor` so that it points to `InfoBoxInputColor` instead.
{% endhint %}

### Kernel no longer caches background and foreground colors

{% code title="KernelColorTools.cs" lineNumbers="true" %}
```csharp
public static Color CurrentForegroundColor
public static Color CurrentBackgroundColor
```
{% endcode %}

{% code title="ConsoleWrapper.cs" lineNumbers="true" %}
```csharp
public static ConsoleColor ForegroundColor
public static ConsoleColor BackgroundColor
```
{% endcode %}

{% code title="IConsoleDriver.cs" lineNumbers="true" %}
```csharp
ConsoleColor ForegroundColor { get; set; }
ConsoleColor BackgroundColor { get; set; }
```
{% endcode %}

The color caching was used as a way to speed up the color querying. However, as performance improvements to different areas of the kernel were witnessed during the third beta development, we've decided to scrap the color caching feature, resulting in the removal of both the `CurrentForegroundColor` and `CurrentBackgroundColor` properties.

Also, to promote the usage of the modern ways to set the background and the foreground color using `ConsoleColor` as a result of Terminaux's `Color` fully supporting `ConsoleColor`, we've decided to remove all the console wrappers for setting the background and the foreground colors.

{% hint style="info" %}
You must replace both wrappers with `SetConsoleColor()` prior to writing text plainly. Otherwise, usage of the built-in console writers is more appropriate and easier.
{% endhint %}

### Number sorting tools moved

{% code title="SortingDriver.cs" lineNumbers="true" %}
```csharp
public static class SortingDriver
```
{% endcode %}

{% hint style="info" %}
Although this driver didn't appear on Beta 2, we've added this in case your mod supports a particular development version of Beta 3 by targeting mod API `3.0.25.282` or lower.
{% endhint %}

We've created `ArrayTools` to put all the useful functions for arrays in one class. As a result, we've had to move all the functions inside `SortingDriver` to that class.

{% hint style="info" %}
None of the functions are changed. You must change the `ArrayTools` references to point to `SortingDriver` instead.
{% endhint %}

### New ways of manipulating objects in the JSON shell

{% code title="JsonTools.cs" lineNumbers="true" %}
```csharp
public static JToken GetProperty(string Property)
public static JToken GetPropertySafe(string ParentProperty, string Property)
```
{% endcode %}

The `Add()` and `Remove()` functions for each object type had been troublesome for a very long time and were error-prone, because of how they were coded. This was a problem with the JSON shell that's absolutely affecting its maintenance.

The solution was to implement brand new functions, `Add()`, `Set()`, and `Remove()`, that allow you to add objects, arrays, or properties to your JSON file at any location you choose.

{% hint style="warning" %}
The old `Add()` and `Remove()` functions were made obsolete as a result of the malleability of the three new functions. It's recommended to use the new functions.
{% endhint %}

### Infobox classes moved to `Inputs`

{% code title="InfoBox*.cs" lineNumbers="true" %}
```csharp
namespace KS.ConsoleBase.Writers.FancyWriters
```
{% endcode %}

The informational box classes used to reside in the FancyWriters part of the console writer. However, when we added several input methods to the information box class, we've discovered that this feature was not actually one of the writers, but one of the input methods.

To clear up the confusion, we've decided to move all the info box classes to the `Inputs.Styles` namespace.

{% hint style="info" %}
None of the classes and their functions were affected, but you must change the imports clause to point to the new namespace, `KS.ConsoleBase.Inputs.Styles`.
{% endhint %}

### Removal of delta-based refresh in the interactive TUI

{% code title="BaseInteractiveTui.cs" lineNumbers="true" %}
```csharp
public static bool RedrawRequired { get; set; } = true;
public virtual bool FastRefresh => true;
```
{% endcode %}

{% code title="IInteractiveTui.cs" lineNumbers="true" %}
```csharp
public bool FastRefresh { get; }
```
{% endcode %}

{% code title="InteractiveTuiTools.cs" lineNumbers="true" %}
```csharp
public static void ForceRefreshSelection()
```
{% endcode %}

The interactive TUI came with the built-in refresh mode that was, at the time, slow on Linux systems. We've attempted to rectify this by implementing the delta-based refresh, but recent improvements have caused slowdowns.

The interactive TUI now uses the Screen feature that's available starting from 0.1.0 Beta 3, which means that the delta-based search has to be eliminated in favor of the speed improvements that the Screen feature brings, along with the malleability regarding the console resizes.

To learn more about the Screen feature, visit the link below:

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}

{% hint style="warning" %}
Remove all calls and overrides to the above removed functions, because they no longer exist. The interactive TUI now internally uses the Screen feature, which means that it has not only become faster, but it has become more resilient when it comes to console resizes.
{% endhint %}

### Adaptive hex and text editor TUIs

{% code title="HexEditorBinding.cs" lineNumbers="true" %}
```csharp
public Action Action { get; }
```
{% endcode %}

{% code title="TextEditorBinding.cs" lineNumbers="true" %}
```csharp
public Action Action { get; }
```
{% endcode %}

{% hint style="info" %}
Although this feature is not introduced in the second beta version of Nitrocid KS 0.1.0, we've added this change for beta testers who produce mods that work on development trunk versions.
{% endhint %}

The hex and the text editor interactive TUIs have recently undergone an improvement regarding the flexibility of opening any file other than their respective shells, causing this breaking change to break all mods that target `3.0.25.304` or lower.

{% hint style="info" %}
The `Action`s have changed to take a function delegate with the array of bytes or strings as input and as output.

```csharp
// Hex
public Func<byte[], byte[]> Action { get; }

// Text
public Func<List<string>, List<string>> Action { get; }
```
{% endhint %}

### Renamed `CloseTextFile()` in `JsonTools`

{% code title="JsonTools.cs" lineNumbers="true" %}
```csharp
public static bool CloseTextFile()
```
{% endcode %}

This was a development overlook when we were implementing the JSON shell tools. Now that we've finally spotted the mistake in naming, we've changed its name to `CloseJsonFile()` to clear up confusion.

{% hint style="info" %}
This function's name has changed to the above name. Its behavior is not changed. You should update all references to the `CloseTextFile()` function to point to the new name.
{% endhint %}

### Remote chat tools renamed

{% code title="RemoteChat.cs" lineNumbers="true" %}
```csharp
namespace KS.Kernel.Debugging.RemoteDebug
{
    public static class RemoteChat
```
{% endcode %}

{% hint style="info" %}
Although this feature is not introduced in the second beta version of Nitrocid KS 0.1.0, we've added this change for beta testers who produce mods that work on development trunk versions.
{% endhint %}

Its namespace needed updating, but the problem was that the class was named in the same name as the namespace in which we're going to update, so we've renamed this class to `RemoteChatTools`.

{% hint style="info" %}
This change breaks all mods that target version `3.0.25.307` or lower. You need to update the namespace imports to `KS.Kernel.Debugging.RemoteDebug.RemoteChat` and the class references to `RemoteChatTools`.
{% endhint %}

### JSON and SQL shell became separate addons!

{% code title="JsonTools.cs" lineNumbers="true" %}
```csharp
public static class JsonTools
```
{% endcode %}

As part of the JSON and SQL shell being addons, we've decided to make adjustments to some of the JSON shell tools so that the ones that are relevant to the JSON shell are put to its own tools, while the beautification and the minification of the JSON files stayed in Nitrocid's JSON tools.

However, we felt that this class needed a movement because of the separation, so we've moved this class to the `KS.Misc.Text` namespace.

{% hint style="info" %}
You can still use the JSON beautification and minification tools if you update the imports to point to the `KS.Misc.Text` namespace.
{% endhint %}

### Screen and Splashes

{% code title="ISplash.cs" lineNumbers="true" %}
```csharp
void Opening(SplashContext context);
void Display(SplashContext context);
void Closing(SplashContext context);
void Report(int Progress, string ProgressReport, params object[] Vars);
void ReportWarning(int Progress, string WarningReport, Exception ExceptionInfo, params object[] Vars);
void ReportError(int Progress, string ErrorReport, Exception ExceptionInfo, params object[] Vars);
```
{% endcode %}

{% code title="BaseSplash.cs" lineNumbers="true" %}
```csharp
public virtual void Opening(SplashContext context)
public virtual void Display(SplashContext context)
public virtual void Closing(SplashContext context)
public virtual void Report(int Progress, string ProgressReport, params object[] Vars)
public virtual void ReportWarning(int Progress, string WarningReport, Exception ExceptionInfo, params object[] Vars)
public virtual void ReportError(int Progress, string ErrorReport, Exception ExceptionInfo, params object[] Vars)
```
{% endcode %}

The splashes were unresponsive to the console resize, except when written to respond to such events, making most of them naturally unresizable.

The screen feature that was introduced in Nitrocid KS 0.1.0 Beta 3 allowed us to implement naturally resizable splash screens that refresh themselves upon resize.

{% hint style="info" %}
The splash code must update their overrides to use the new signatures:

```csharp
public virtual string Opening(SplashContext context)
public virtual string Display(SplashContext context)
public virtual string Closing(SplashContext context, out bool delayRequired)
public virtual string Report(int Progress, string ProgressReport, params object[] Vars)
public virtual string ReportWarning(int Progress, string WarningReport, Exception ExceptionInfo, params object[] Vars)
public virtual string ReportError(int Progress, string ErrorReport, Exception ExceptionInfo, params object[] Vars)
```

Afterwards, you must update the code in all of the overrides (if any) so that it builds a string full of VT sequences and text to print to the console. For example, the [Welcome splash screen](https://github.com/Aptivi/NitrocidKS/commit/6e05dcf86e3706265afaf4220b1a28d991c9df05#diff-bf50d9b8737e56449d43e3d0a56e42174f24e498bd0cbc6b7c2e6eca2578dbad) has been adjusted to use this feature.
{% endhint %}

### Implemented `ArgumentParameters`

{% code title="ArgumentExecutor.cs" lineNumbers="true" %}
```csharp
public virtual void Execute(string StringArgs, string[] ListArgsOnly, string[] ListSwitchesOnly)
```
{% endcode %}

{% code title="IArgument.cs" lineNumbers="true" %}
```csharp
void Execute(string StringArgs, string[] ListArgsOnly, string[] ListSwitchesOnly);
```
{% endcode %}

The `ArgumentParameters` class was implemented to group the variables intended to show argument and switch lists. This is to avoid having to change the signature of the whole `Execute()` method every single addition or removal is planned.

{% hint style="info" %}
The `Execute()` signature has been changed, so you need to update all your inherited argument classes to hold the new signature:

```csharp
public override void Execute(ArgumentParameters parameters)
```

Implemented by:

```csharp
void Execute(ArgumentParameters parameters);
```
{% endhint %}

### Implemented `RemoteDebugCommandParameters`

{% code title="IRemoteDebugCommand.cs" lineNumbers="true" %}
```csharp
void Execute(string StringArgs, string[] ListArgsOnly, string[] ListSwitchesOnly, RemoteDebugDevice device);
```
{% endcode %}

{% code title="RemoteDebugBaseCommand.cs" lineNumbers="true" %}
```csharp
public virtual void Execute(string StringArgs, string[] ListArgsOnly, string[] ListSwitchesOnly, RemoteDebugDevice device)
```
{% endcode %}

The `RemoteDebugCommandParameters` class was implemented to group the variables intended to show argument and switch lists. This is to avoid having to change the signature of the whole `Execute()` method every single addition or removal is planned.

{% hint style="info" %}
The `Execute()` signature has been changed, so you need to update all your inherited remote debug command classes to hold the new signature:

```csharp
public override void Execute(RemoteDebugCommandParameters parameters, RemoteDebugDevice device)
```

Implemented by:

```csharp
void Execute(RemoteDebugCommandParameters parameters, RemoteDebugDevice device);
```
{% endhint %}

### Moved `KernelPathType` to `Files.Paths`

{% code title="KernelPathType.cs" lineNumbers="true" %}
```csharp
public enum KernelPathType
```
{% endcode %}

We've moved `KernelPathType` to its own namespace under the `Files` namespace, called `Paths`, since the custom paths got implemented.

With this change, we've also moved the following classes to `Files.Paths`:

* `Paths` -> `PathsManagement`
* `PathLookupTools`

{% hint style="info" %}
For calls to `Paths`, you need to reference it again under the new class name, `PathsManagement`.

For the rest of the classes, you need to update the namespace imports to `Files.Paths`.
{% endhint %}

### Moved input modes to their own namespaces

```csharp
// Choice*, InfoBox*, and Selection*
namespace KS.ConsoleBase.Inputs.Styles
```

The three input modes have been moved to their own namespaces, because we wanted to organize the `KS.ConsoleBase.Inputs.Styles` namespace to be clean in case we add more input modes.

The following three input modes are affected:

* `ChoiceStyle` -> `KS.ConsoleBase.Inputs.Styles.Choice`
  * Its accompanying `ChoiceOutputType` has been moved to the above namespace
* `InfoBoxButtonsColor` -> `KS.ConsoleBase.Inputs.Styles.Infobox`
* `InfoBoxColor` -> `KS.ConsoleBase.Inputs.Styles.Infobox`
* `InfoBoxInputColor` -> `KS.ConsoleBase.Inputs.Styles.Infobox`
* `InfoBoxProgressColor` -> `KS.ConsoleBase.Inputs.Styles.Infobox`
* `InfoBoxSelectionColor` -> `KS.ConsoleBase.Inputs.Styles.Infobox`
* `InfoBoxSelectionMultiColor` -> `KS.ConsoleBase.Inputs.Styles.Infobox`
* `SelectionMultipleStyle` -> `KS.ConsoleBase.Inputs.Styles.Selection`
* `SelectionStyle` -> `KS.ConsoleBase.Inputs.Styles.Selection`

{% hint style="info" %}
The above input modes have been moved to the above namespaces, so you need to update your namespace imports to reference the above namespaces.
{% endhint %}

### Moved advanced diagnostics to its own addon

{% code title="ThreadManager.cs" lineNumbers="true" %}
```csharp
public static Dictionary<string, string[]> GetThreadBacktraces()
```
{% endcode %}

This function used to use an advanced diagnostics package, called ClrMd, that facilitates the diagnostics by providing tools that are effective, such as getting all the thread backtraces.

Because the entire kernel debugging system, except a very small part of it, used such diagnostics, we've moved such tools to its own Extras addon. This is not done in an effort to attempt to reduce dependencies to its bare minimum, and may be reinstated in a future release post-0.1.0.

{% hint style="info" %}
This function has not been removed, but it has been modified to call the same function using the inter-addon communication recently implemented in the third beta version of 0.1.0.

When your mod calls this function, be sure to handle an extra case where it returns an empty dictionary.
{% endhint %}

### SpecProbe updated to 1.2.0

{% hint style="info" %}
This is not exactly an API-related breaking change, but this is a breaking change for Windows users. Linux and macOS are not affected.
{% endhint %}

When SpecProbe was updated to 1.2.0, it contained a re-written hard disk prober that gets all the hard drives and their partitions without relying on [`DriveInfo.GetDrives()`](https://learn.microsoft.com/en-us/dotnet/api/system.io.driveinfo.getdrives) to get all the mounted drives. As a result, it returned a more complete partition list in your hard drives if you're running Windows.

However, this re-written hard disk prober requires administrative rights, because it calls [`DeviceIoControl()`](https://learn.microsoft.com/en-us/windows/win32/api/ioapiset/nf-ioapiset-deviceiocontrol), which is considered a powerful function for device I/O controls, such as getting drive geometry, getting drive partition table information, and so on. That function was used to directly talk to your drive for such information, which is why it requires administrator rights.

**As a result, Nitrocid KS 0.1.0 Beta 3 will start requiring administrative privileges, starting from commit** [**`dda1d6d`**](https://github.com/Aptivi/NitrocidKS/commit/dda1d6d1d7209682c30b9b93d053056fad40cdfa)**. However, the final version won't ask for administrative privileges unless you explicitly tell it to reboot to the elevated mode on Windows systems.**

### Renamed `RemovePostfix()` to `RemoveSuffix()`

{% code title="TextTools.cs" lineNumbers="true" %}
```csharp
public static string RemovePostfix(this string text, string postfix)
```
{% endcode %}

It has been recently discovered that "postfix" is less understandable than "suffix" for some of the users, so we've adjusted the name of the function to better align with these requirements.

{% hint style="info" %}
The functionality of this function wasn't changed while renaming this function.
{% endhint %}

### Removed hidden commands

{% code title="CommandFlags.cs" lineNumbers="true" %}
```csharp
public enum CommandFlags
{
    (...)
    Hidden = 64
}
```
{% endcode %}

Normally, the hidden commands serve as a way to hide a selected command that may have a secret in it. However, it is not completely hidden, because mods can be reverse-engineered using the .NET reverse engineering tools to get the name of the command, which causes this command to be no longer hidden.

We've decided to remove this feature for this reason.

{% hint style="danger" %}
We advice you to stop using this feature.
{% endhint %}

### Removed mod parts

{% code title="ModInfo.cs" lineNumbers="true" %}
```csharp
internal Dictionary<string, ModPartInfo> ModParts { get; set; }
```
{% endcode %}

{% code title="ModPartInfo.cs" lineNumbers="true" %}
```csharp
public class ModPartInfo
```
{% endcode %}

Mod parts were historically implemented as a way to group parts of one mod when they were just a single C# source code file. That was a way to group several parts of a mod to achieve their own goal.

The problem was that we've taken out support for such mods and replaced them with DLL-based mods, which are still used at this moment because they don't require dynamically compiling the source code with CodeDom. As a result, we've negated the role of CodeDom in terms of loading mods and their so-called "parts".

Upon further examination as to how the mods are loaded, we've reached to a conclusion that mod parts went useless as more and more improvements went in the way to the mod loading system. This feature has now become a maintenance burden when it comes to maintaining the mod system, especially the part where we would finalize the mod for a specific mod. Recent changes to the modding system have also concluded that mod parts are just categorical sugar.

As a result, we've decided to finally remove the mod parts as their role has been cancelled by DLL-based mods and as C# source code-based mods were long removed from the mod loading system to get rid of CodeDom.

{% hint style="info" %}
This only breaks your mod manager code as it only changes the kernel mod information class and not the mod interface itself. Your mods should still work properly.
{% endhint %}

### Moved permissions to `Security`

{% code title="PermissionsTools.cs" lineNumbers="true" %}
```csharp
namespace KS.Users.Permissions
```
{% endcode %}

The user permissions feature was implemented as a way to control what users can and cannot do. As a result, it's considered as a security feature more than it's a user accounts feature. Hence, the feature needs to be moved to the `Security` namespace.

{% hint style="info" %}
None of the functions have been changed as a result of this move. You just need to update your imports to `KS.Security.Permissions`.
{% endhint %}

### Notification handling changes

Progress notification's handling has changed, because we no longer look for the progress percentage. We only look for the state now to accurately reflect the success or the failure.

{% hint style="info" %}
As a result, your progress notifications have to change their state to either a success or a failure, even when their progress property went to 100%. You can simply do this:

```csharp
Notif.ProgressState = NotificationProgressState.Success;
```
{% endhint %}

### Improved custom language loading functions

{% code title="LanguageManager.cs" lineNumbers="true" %}
```csharp
public static void InstallCustomLanguage(string LanguageName, bool ThrowOnAlreadyInstalled = true)
```
{% endcode %}

`InstallCustomLanguage()` used to only handle languages that have been installed to the custom languages path located in the application data folder. Over time, during the entire span of development, we've noticed that this feature was the least tested feature, so we've decided to take care of it.

By taking care of it, we'd agreed that we'd change the `InstallCustomLanguage()` function so that it takes a path to the JSON file of a language that isn't installed to the kernel yet, and that we'd make a new function that derives from it, called `InstallCustomLanguageByName()`, to preserve the old behavior of pre-Beta 3 versions.

{% hint style="info" %}
If you still want to use the language name, you need to change the function so that it calls its `...ByName()` sibling instead of the primary one, which now takes the path.
{% endhint %}

### Moved `KernelColorType` versions of writers

{% code title="All console writers" lineNumbers="true" %}
```csharp
public static void WriteKernelColor(string Text, KernelColorType colorType, params object[] vars)
public static void WriteKernelColor(string Text, bool Line, KernelColorType colorType, params object[] vars)
public static void WriteKernelColor(string Text, bool Line, bool Highlight, KernelColorType colorType, params object[] vars)
(...)
```
{% endcode %}

Because the `KernelColorType` version of all console writers are not part of Terminaux's standard console writers, we've decided to isolate this version of these functions that write text to console in different styles so the following classes house them:

* `TextWriters`: Standard console writing, including positioning and slow writing.
* `TextFancyWriters`: Fancy console writing, including Figlet writing and simple console graphics.
* `TextMiscWriters`: Miscellaneous console writing, including line with handle writer.

This change was done as a preparation for the deduplication effort happening as soon as the release candidate cycle starts.

{% hint style="info" %}
If you wish to use the above `KernelColorType`-based writers, you need to change the class reference to one of the three above classes. Here are three examples:

* `TextWriterColor.WriteKernelColor` -> `TextWriters.Write`
* `FigletColor.WriteFigletKernelColor` -> `TextFancyWriters.WriteFiglet`
* `ListWriterColor.WriteList` -> `TextWriters.WriteList`
{% endhint %}

### Terminaux 2.0 migration changes

Nitrocid KS 0.1.0 Beta 3 uses Terminaux 2.0 to handle color work. As a result, we need you to consult the changelogs for this version of Terminaux here:

{% content-ref url="https://app.gitbook.com/s/G0KrE9Uk2AiblqjWtpAo/breaking-changes/api-v2.0" %}
[API v2.0](https://app.gitbook.com/s/G0KrE9Uk2AiblqjWtpAo/breaking-changes/api-v2.0)
{% endcontent-ref %}
