---
description: Guide for upgrading 0.1.0 Beta 1 mods to 0.1.0 Beta 2
---

# ⬆️ From 0.1.0 Beta 1 to 0.1.0 Beta 2

This page lists all the changes that have been made from 0.1.0 Beta 1 to 0.1.0 Beta 2. For upgrading your mods from 0.0.24.x directly to the 0.1.0 series, use the main upgrade page where it highlights the most important changes.

## From Beta 1 to Beta 2

During Beta 2's development, we have made the following breaking changes:

### Employed `ColorPrint.Core` for color wheel

{% code title="ColorWheelOpen.vb" lineNumbers="true" %}
```visual-basic
Namespace ConsoleBase.Colors
    Public Module ColorWheelOpen
```
{% endcode %}

The color wheel was implemented in the kernel to allow color selection. However, it lacked features such as the color blindness simulation, so we decided to make an entirely new library dedicated to this. As a result, we've replaced the kernel's implementation with `ColorPrint.Core`, which is more portable and easier to use.

{% hint style="info" %}
Merge your calls to the `ColorWheelOpen` class to `ColorPrint.Core` as it's easier to use and more portable than the former class.
{% endhint %}

### Removed the country-based RSS feed selector

{% code title="RSSTools.cs" lineNumbers="true" %}
```csharp
public static void OpenFeedSelector()
```
{% endcode %}

The feed selector used to prompt you for a country so that you can select a feed according to the minimal list of known RSS feeds operating from the selected country. Recently, we've discovered many broken links (404's) across many feeds, so we decided to remove it as the source was out of date.

{% hint style="warning" %}
Its functionality will be replaced with the bookmarked feed selector. Meanwhile, it's advisable to stop using this function on your mods.
{% endhint %}

### Removed `UseCtrlCAsInput` from `ReadLine()`'s

{% code title="Input.cs" lineNumbers="true" %}
```csharp
public static string ReadLine(bool UseCtrlCAsInput)
public static string ReadLine(string InputText, string DefaultValue, bool UseCtrlCAsInput)
public static string ReadLineUnsafe(string InputText, string DefaultValue, bool UseCtrlCAsInput)
```
{% endcode %}

`UseCtrlCAsInput` used to tell ReadLine.Reboot to treat `Ctrl + C` as input to try to aid in cancelling prompts. Apparently, that library was deprecated and replaced with TermRead. As a result, this feature is removed.

{% hint style="danger" %}
We advice you to cease using this variable in the above functions.
{% endhint %}

### Removed `SaveCurrDir()`

{% code title="CurrentDirectory.cs" lineNumbers="true" %}
```csharp
public static void SaveCurrDir()
public static bool TrySaveCurrDir()
```
{% endcode %}

This function used to save the current directory value to the kernel configuration file stored in the `%LOCALAPPDATA%/KS/KernelMainConfig.json` for Windows or the `$HOME/.config/ks/KernelMainConfig.json`. Following the recent improvements in the configuration system that took place two times during 0.1.0's development, the `SaveCurrDir()` function was effectively reduced to a single call to the `Config.CreateConfig()` function, which already deals with this kind of change, rendering this function useless. As a result, we decided to remove this function as it added no value and was just an extraneous wrapper function.

{% hint style="warning" %}
If you want to save the current directory value in the future, please use the kernel settings application to save the configuration. Programmatically, you'll have to call the `Config.CreateConfig()` function instead of the two above functions.
{% endhint %}

### Group system and the User Management API

{% code title="UserManagement.cs" lineNumbers="true" %}
```csharp
public static void LoadUserToken()
public static JToken GetUserProperty(string User, UserProperty PropertyType)
public static void SetUserProperty(string User, UserProperty PropertyType, JToken Value)
public static void InitializeSystemAccount()
```
{% endcode %}

We've added a real group system to the KS.Users namespace under Groups in order to group the users and give them shared permissions. In consequence, we had to rework the user management system, causing several API removals.

The following functions were documented:

* `LoadUserToken()`: Tries to manually deserialize the user configuration JSON file to be usable with all the user management functions.
* `InitializeSystemAccount()`: Tries to initialize a root account with a defined password from the same user token.
* `GetUserProperty()`: Gets a user property as a JSON token, JToken, which has to be manually cast to either a string, boolean, or array of strings.
* `SetUserProperty()`: Sets a user property, taking an uncasted JSON token that represents either a string, boolean, or array of strings.

The first two functions were removed, because they were deemed to be unnecessary for a long time. A re-work to the user management API made it reasonable to remove these functions.

However, the last two functions were removed because it was found to be complicated for having to cast to their appropriate types manually. We also removed the ability to directly set a user's properties security-wise.

{% hint style="danger" %}
Because the user management API reached to a stage when it was re-written to simplify things, we advice you to cease using these functions.
{% endhint %}

### Initial implementation of the TUI

The following classes are affected:

{% code lineNumbers="true" %}
```csharp
public static class ContactsManagerCli
public static class TaskManagerCli
public static class FileManagerCli
```
{% endcode %}

The following functions are affected:

{% code lineNumbers="true" %}
```csharp
public static void OpenMain()
```
{% endcode %}

The three CLIs used to implement their own CLI based on data for contacts, kernel threads, and files respectively. Now, with the introduction of the `IInteractiveTui` interface and the `BaseInteractiveTui` class, we've made the three above classes implement both the classes, removing the `static` modifier from the three CLIs.

As a result, we've removed the `OpenMain()` function from all of them. However, you can still open the CLI as follows:

{% code lineNumbers="true" %}
```csharp
InteractiveTuiTools.OpenInteractiveTui(new ContactsManagerCli());
InteractiveTuiTools.OpenInteractiveTui(new TaskManagerCli());
InteractiveTuiTools.OpenInteractiveTui(new FileManagerCli());
```
{% endcode %}

This removal was necessary to avoid code repetition, effectively making maintenance easier.

{% hint style="info" %}
If you want to open the three CLIs, refer to the above code snippet about the usage of `OpenInteractiveTui()`.
{% endhint %}

### Added warning reports to the splash interface

{% code title="ISplash.cs" lineNumbers="true" %}
```csharp
void ReportWarning(int Progress, string WarningReport, Exception ExceptionInfo, params object[] Vars);
```
{% endcode %}

The splash reporter used to only report the progress and the error messages during the kernel startup. Now, we've added warnings to extend the possibility of accurately reporting warnings without having to rely on special conditions on the progress report.

{% hint style="info" %}
Mods that implement splashes now have to implement the above function to be able to report warnings.
{% endhint %}

### Added `ReadToEndAndSeek()` to filesystem driver

{% code title="IFilesystemDriver.cs" lineNumbers="true" %}
```csharp
string ReadToEndAndSeek(ref StreamReader stream);
```
{% endcode %}

As part of the removal of Extensification dependency from Nitrocid KS as a result of its deprecation, we decided to adapt the kernel to remove all references to Extensification and re-implement things. The commit states:

> Since this library is deprecated starting today, we've removed all references to Extensification by updating all libraries and making necessary changes to the API to restore the used Extensification functions to `TextTools`, `IntegerTools`, and `StreamRead`. Custom filesystem drivers in your mods will now have to implement `ReadToEndAndSeek()` to be compatible with this revision of N-KS, so we've increased the mod API version to `3.0.25.7`.

{% hint style="info" %}
Mods that implement filesystem drivers now have to implement the above function to be able to be installed on N-KS mod API 3.0.25.7 or later.
{% endhint %}

### Removed `ConsoleWrapper.WindowTop`

{% code title="ConsoleWrapper.cs" lineNumbers="true" %}
```csharp
public static int WindowTop
```
{% endcode %}

`Console.WindowTop` is always 0 on both the Windows and the Unix operating systems, but you can set this value only on Windows systems, which kills the potential of cross-platform. This is deemed unnecessary for most of the things, which makes it redundant, so we've removed this function from both the `ConsoleWrapper` and the `Console` driver.

{% hint style="danger" %}
We advice you to replace all instances of this function from `ConsoleWrapper` with `0`, and remove all implementations of this property from all your `Console` drivers.
{% endhint %}

### Converted string char variables to chars

{% code lineNumbers="true" %}
```csharp
public string CustomUpperLeftCornerChar { get; set; } = "╔";
public string CustomUpperRightCornerChar { get; set; } = "╗";
public string CustomLowerLeftCornerChar { get; set; } = "╚";
(...)
```
{% endcode %}

The above char variables used to hold only one character, but they were defined as strings instead of chars, so users made it possible to place more than one character to these variables. Usually, the handlers only take the first character from the string.

Since the config entries defined these variables as configurable under the `SChar` type, the config app made enough checking to make sure that only one character can be input. This caused us to convert all these variables to strings, affecting the following classes:

* `NotificationManager`
* `Notification`
* `BarRotSettings`
* `IndeterminateSettings`
* `ProgressClockSettings`
* `RampSettings`
* `BorderTools`
* `ProgressTools`

The following writers are also affected and are found to be using characters as strings before the change:

* `WriteBorder()` and `WriteBorderPlain()`
* `WriteBoxFrame()` and `WriteBoxFramePlain()`
* `WriteInfoBox()` and `WriteInfoBoxPlain()`
* `WriteProgress()` and `WriteProgressPlain()`
* `WriteVerticalProgress()` and `WriteVerticalProgressPlain()`

{% hint style="info" %}
For mods that treat characters as strings and use one of the above classes and variables that hold only one character, you'll have to refactor the logic to treat these characters as chars. However, if you use char variables and convert them to string in either the writers or the char variables, such as `CustomLowerLeftCornerChar`, you'll no longer need to convert your char variables to strings.
{% endhint %}

### Added finding files using regular expressions

The following functions are added to the filesystem driver:

{% code title="IFilesystemDriver.cs" lineNumbers="true" %}
```csharp
string[] GetFilesystemEntriesRegex(string Parent, string Pattern, bool Recursive = false);
```
{% endcode %}

This function gets all the filesystem entries found under the specified parent directory, with the regular expression pattern. This uses the regular expression driver to handle the regex work.

{% hint style="info" %}
Mods that implement filesystem drivers now have to implement the above function to be able to use it. It's also advisable to use the regex driver in the function implementation.
{% endhint %}

### Removed configurable maintenance mode

{% code title="Flags.cs" lineNumbers="true" %}
```csharp
public static bool Maintenance
```
{% endcode %}

Long ago, we used to allow users to force the kernel to maintenance mode upon setting this value right from the configuration application. However, this can be abused, so we decided to remove it as a configuration entry, but the mode itself stays. You can still run the kernel in maintenance mode by passing `maintenance` as a command line argument to Nitrocid's entry point. Refer to the below page for more information:

{% content-ref url="../../../advanced-and-power-users/inner-workings/inner-essentials/kernel-arguments.md" %}
[kernel-arguments.md](../../../advanced-and-power-users/inner-workings/inner-essentials/kernel-arguments.md)
{% endcontent-ref %}

{% hint style="danger" %}
We advice you to cease using this flag. Maintenance mode doesn't allow mods to load, anyways.
{% endhint %}

### Migrated two speed dial types to one JSON file

{% code title="Paths.cs" lineNumbers="true" %}
```csharp
public static string FTPSpeedDialPath
public static string SFTPSpeedDialPath
```
{% endcode %}

{% code title="PathType.cs" lineNumbers="true" %}
```csharp
/// <summary>
/// FTP speed dial file.
/// </summary>
FTPSpeedDial,
/// <summary>
/// SFTP speed dial file.
/// </summary>
SFTPSpeedDial,
```
{% endcode %}

{% code title="SpeedDialTools.cs" lineNumbers="true" %}
```csharp
public static KernelPathType GetPathTypeFromSpeedDialType(SpeedDialType SpeedDialType)
public static JObject GetTokenFromSpeedDial(SpeedDialType SpeedDialType)
public static Dictionary<string, JToken> ListSpeedDialEntries(SpeedDialType SpeedDialType)
public static JToken GetQuickConnectInfo(SpeedDialType SpeedDialType)
```
{% endcode %}

To migrate two different JSON files holding speed dial entries for FTP and SFTP servers, we decided to replace `FTPSpeedDial` and `SFTPSpeedDial` with `SpeedDial` and `FTPSpeedDialPath` and `SFTPSpeedDialPath` with `SpeedDialPath`, effectively removing a function from `SpeedDialTools`, `GetPathTypeFromSpeedDialType()`, and the remaining functions removing the `(SpeedDialType SpeedDialType)` signature, causing them not to take any speed dial type.

The commit said:

> This migration is necessary to efficiently put speed dials to one JSON file as we have the Type key already in speed dial properties.

{% hint style="info" %}
We advice you to replace all calls to type-specific enumerations and paths with a single speed dial enumeration value, `SpeedDial`, and path, `SpeedDialPath`, respectively. Also, we advice you to use the three functions that stay, but without the type, and cease using the `GetPathTypeFromSpeedDialType()` function.
{% endhint %}

### Added support for `SwitchInfo`

{% code title="CommandArgumentInfo.cs" lineNumbers="true" %}
```csharp
public HelpUsage[] HelpUsages { get; private set; }
public CommandArgumentInfo(string[] HelpUsages, bool ArgumentsRequired, int MinimumArguments, Func<string, int, char[], string[]> AutoCompleter = null)
public CommandArgumentInfo(HelpUsage[] HelpUsages, bool ArgumentsRequired, int MinimumArguments, Func<string, int, char[], string[]> AutoCompleter = null)
```
{% endcode %}

{% code title="HelpUsage.cs" lineNumbers="true" %}
```csharp
public class HelpUsage
```
{% endcode %}

`CommandArgumentInfo` and the UESH shell used to hold `HelpUsage` classes to get info about help usages, but they didn't become aware of unknown switches, so we decided to split `HelpUsage` to a string of command arguments and command switches, using `SwitchInfo` as a class to hold help information for each switch, including the properties for each switch, like the requirement and the value requirement.

This causes us to remove all help helpers that hold info about the supported switches, especially since the later commands that do support switches didn't have their help helpers that tell the users about the command switches.

In consequence, `CommandArgumentInfo` has been changed to hold only one constructor that holds both array of arguments and array of switch information class instances.

{% hint style="info" %}
We advice you to use the below constructor to accommodate the above changes made:

```csharp
public CommandArgumentInfo(string[] Arguments, SwitchInfo[] Switches, bool ArgumentsRequired, int MinimumArguments, bool AcceptsSet = false, Func<string, int, char[], string[]> AutoCompleter = null)
```
{% endhint %}

An example is provided below:

{% code title="Snippet from UESHShellInfo.cs" lineNumbers="true" %}
```csharp
{ "find",
    new CommandInfo("find", ShellType, /* Localizable */ "Finds a file in the specified directory or in the current directory",
        new[] {
            new CommandArgumentInfo(new[] { "file", "directory" }, new[] {
                new SwitchInfo("recursive", /* Localizable */ "Searches for a file recursively", false, false, Array.Empty<string>(), 0, false),
                new SwitchInfo("exec", /* Localizable */ "Executes a command on a file", false, true)
            }, true, 1, true)
        }, new FindCommand())
},
```
{% endcode %}

### Migrated text tools

{% code title="Deleted classes" lineNumbers="true" %}
```csharp
public static class StringManipulate
public static class StringQuery
```
{% endcode %}

As `TextTools` matured with different string tools, we decided to migrate the following functions from their respective classes to that class under the `KS.Misc.Text` namespace:

* `FormatString(string Format, params object[] Vars)`
* `IsStringNumeric(string Expression)`

{% hint style="info" %}
As a result, we advice you to use the `TextTools` class to execute the above two functions. Their signatures and behavior remains unchanged. This means removing any reference to the two classes found under the `KS.Misc.Reflection` namespace.
{% endhint %}

### Moved `BannerFigletFont` to `WelcomeMessage`

{% code title="KernelTools.cs" lineNumbers="true" %}
```csharp
public static string BannerFigletFont =>
    Config.MainConfig.BannerFigletFont;
```
{% endcode %}

As this property doesn't have to do with the kernel tools, we decided to move from the `KernelTools` class to the `WelcomeMessage` class found under the `KS.Misc.Writers.MiscWriters` namespace.

{% hint style="info" %}
If you want to continue using this property, we suggest that you change the `using KS.Kernel` line to replace the namespace with the `KS.Misc.Writers.MiscWriters` namespace in order to be able to reference `BannerFigletFont` under `WelcomeMessage`.
{% endhint %}

### Easier way to render info into the second pane

{% code title="IInteractiveTui.cs" lineNumbers="true" %}
```csharp
public string RenderInfoOnSecondPane(object item)
```
{% endcode %}

When we first introduced the interactive TUI feature, we had to leave the entire "rendering to the second pane" logic to your interactive TUI instance. However, since this involves having to wrap the informational pane and to work out the text lengths and console positions yourself, we've placed these requirements so that it's up to the handler to handle printing the information, effectively replacing the function above with `GetInfoFromItem`.

{% hint style="info" %}
It's now easier to render the information pane using just your text! Just form your final informational text by implementing the `GetInfoFromItem` function to return said information. The function signature is found below:

```csharp
public string GetInfoFromItem(object item)
```
{% endhint %}

### Enhanced the screen locking feature

{% code title="Login.cs" lineNumbers="true" %}
```csharp
public static void ShowPasswordPrompt(string usernamerequested)
```
{% endcode %}

The screen locking feature was malfunctioning for a long time. Unfortunately, it went under the radar, so it wasn't looked at. We've finally managed to enhance the screen locking feature by checking our heuristics. Moreover, we had to separate the `ShowPasswordPrompt` function from trying to log the user in, as this was the requirement in enhancing the screen lock, since it was used by this feature.

{% hint style="info" %}
The `ShowPasswordPrompt` function now contains a return type of `bool`, which returns `true` if the password was valid or if the user database concluded that the password was empty, and `false` if the password was invalid. The new signature is now:

{% code title="Login.cs" lineNumbers="true" %}
```csharp
public static bool ShowPasswordPrompt(string usernamerequested)
```
{% endcode %}
{% endhint %}

### Moved journaling related classes to `KS.Kernel.Journaling`

{% code title="JournalManager.cs, JournalStatus.cs" lineNumbers="true" %}
```csharp
namespace KS.Kernel.Administration.Journalling
```
{% endcode %}

We used to place all the journaling-related classes and tools to the `KS.Kernel.Administration.Journalling` namespace, until we've found out that said namespace wasn't needed because we didn't have plans to add another namespace to the `Administration` namespace, so we decided to move journaling-related classes to the `KS.Kernel.Journaling` namespace.

{% hint style="info" %}
To continue using these journaling classes, you have to change the `using` statements to point to `KS.Kernel.Journaling`.
{% endhint %}

### Time and Date API enhancements

{% code title="Old TimeDate namespace" lineNumbers="true" %}
```csharp
namespace KS.TimeDate
```
{% endcode %}

The TimeDate API was left untouched for a long time, and its structure was a bit too old for the modern API system that Nitrocid KS uses to be usable for mods and general purpose, so we have decided to create the following namespaces:

* `KS.Kernel.Time.Converters`
* `KS.Kernel.Time.Renderers`
* `KS.Kernel.Time.Timezones`
* `KS.Kernel.Time`

We have moved relevant functions according to their action to the above namespaces. For example, the `Render()` function was moved to `TimeDateRenderers` class in the `Renderers` namespace.

We have also isolated the `FormatType` enumeration from the `TimeDateTools` class so that the mods don't have to reference the class name in order to access this enumeration.

Also, some functions like `GetRemainingTimeFromNow()` have their return types changed from `string` to `TimeSpan`. To get a string, you must now use the `RenderRemainingTimeFromNow()` function to continue using strings to render the remaining time from the current date and time.

{% hint style="info" %}
To keep using these functions, you must now change the imports in relevant code files to `KS.Kernel.Time` and its ancestors to be able to use their classes. You may need to adjust your usage of the functions and/or classes so that the behavior you're expecting remains unchanged.
{% endhint %}

### Manual page parsing now uses the interactive TUI

The manual page viewer used to be so limited in features; you could only advance to the next pages of the manual and exit back to the shell. You had to specify the full manual page title after listing it with the `-list` switch.

The migration of the viewer to the TUI required the following changes:

{% code title="PageManager.cs" lineNumbers="true" %}
```csharp
public static Dictionary<string, Manual> ListAllPages()
public static Dictionary<string, Manual> ListAllPages(string SearchTerm)
```
{% endcode %}

The above functions have their return values changed to a `List<Manual>` type. It was done because we needed to store a kernel modification name inside the `Manual` instance, so we have done the below changes:

{% code title="PageManager.cs and PageParser.cs" lineNumbers="true" %}
```csharp
public static void AddManualPage(string Name, Manual Page)
public static void InitMan(string ManualFile)
```
{% endcode %}

The two above functions have changed their signature so they they take a mod name (`string modName`) before every element.

{% code title="PageManager.cs" lineNumbers="true" %}
```csharp
public static bool RemoveManualPage(string Name)
```
{% endcode %}

The above function has its signature changed so that instead of the manual page name (title), it now takes an instance of the `Manual` class to remove a selected manual page.

{% hint style="info" %}
You should adjust the changes as necessary in order for your mod to continue working.
{% endhint %}

### `ChoiceOutputType` is now standalone

{% code title="ChoiceStyle.cs" lineNumbers="true" %}
```csharp
public enum ChoiceOutputType
```
{% endcode %}

We have separated the `ChoiceOutputType` enumeration from the `ChoiceStyle` class so that we can simplify its access. Now, you can access this enumeration directly; you don't have to append `ChoiceStyle` before the enumeration so that it becomes easier for you to access.

{% hint style="info" %}
You need to remove the ChoiceStyle reference and make it only reference the ChoiceOutputType enumeration so that your mods keep working. For example:

<pre class="language-csharp" data-line-numbers><code class="lang-csharp">// Before
<strong>    var OutputType = ChoiceStyle.ChoiceOutputType.OneLine;
</strong>                     XXXXXXXXXXX
// After
<strong>    var OutputType = ChoiceOutputType.OneLine;
</strong>                     ^^^^^^^^^^^^^^^^
</code></pre>
{% endhint %}

### Breaking changes following the `OpenConnectionForShell()` implementation

{% code title="IShellInfo.cs" lineNumbers="true" %}
```csharp
bool AcceptsNetworkConnection { get; }
```
{% endcode %}

In order for `OpenConnectionForShell()` to be able to distinguish the shells that accept the network connections from the shells that don't, we needed to add the above property to the interface implementation of the `ShellInfo` class. The base class has been updated so that it implements the above property as false so that you don't have to override this property in the non-networked shells implemented either by Nitrocid or by your mods:

{% code title="BaseShellInfo.cs" lineNumbers="true" %}
```csharp
public virtual bool AcceptsNetworkConnection => false;
```
{% endcode %}

{% hint style="info" %}
For most shells that don't connect to any network, you don't have to override the above property. Nitrocid KS 0.1.0 with API versions lower than 3.0.25.34 are unable to use `ShellInfo` instances implemented for such versions.
{% endhint %}

### Renamed Shell to ShellManager

{% code title="Shell.cs" lineNumbers="true" %}
```csharp
public static class Shell
```
{% endcode %}

The shell management module used to have just `Shell` without the `Manager` suffix, because it was introduced on 0.0.1, which was the first version of Kernel Simulator as a whole that released on February 22nd, 2018. You can view the historical representation of the shell management class by clicking on [this link](https://github.com/Aptivi/NitrocidKS/blob/v0.0.1.x-servicing/src/Windows/0.0.1/Kernel%20Simulator/Shell.vb).

However, this naming has introduced problems for us because we needed to write `Shell` twice before being able to write its function names for reference, which is messy. As a result, we had to rename this class to `ShellManager` so that its access is easier.

{% hint style="info" %}
We advice you to change the references to the `Shell` class to use the `ShellManager` class.
{% endhint %}

### `ColorTools` class renamed to `KernelColorTools`

{% code title="ColorTools.cs and ColorSetErrorReasons.cs" lineNumbers="true" %}
```csharp
public static class ColorTools
public enum ColorSetErrorReasons
```
{% endcode %}

The `ColorTools` class used to hold color tools which have to do directly with the kernel and its console driver so that it can manipulate with the colors, like setting the console color, setting the console background color, and so on.

However, it has been recently concluded that there is a class under the same name from the ColorSeq library, which caused us to have to add an extra imports clause to rename the `ColorTools` class so that it doesn't conflict with ColorSeq's class.

We were being fed up of this, so we decided to rename the entire class to `KernelColorTools`.

{% hint style="info" %}
You no longer have to add the imports clause that would rename the abovementioned class. The clause that is to be removed looks like this:

```csharp
using ColorTools = KS.ConsoleBase.Colors.ColorTools
```
{% endhint %}

{% hint style="info" %}
All calls to the kernel's `ColorTools` will now have to be called through the `KernelColorTools` class. Example below:

<pre class="language-csharp" data-line-numbers><code class="lang-csharp">// Before
<strong>internal static readonly Dictionary&#x3C;KernelColorType, Color> SelectedColors = ColorTools.PopulateColorsCurrent();
</strong>                                                                             XXXXXXXXXX
// After
<strong>internal static readonly Dictionary&#x3C;KernelColorType, Color> SelectedColors = KernelColorTools.PopulateColorsCurrent();
</strong>                                                                             ^^^^^^^^^^^^^^^^
</code></pre>
{% endhint %}

### Namespace movements

```csharp
namespace KS.Misc.Execution
namespace KS.Misc.Threading
namespace KS.Misc.Writers
namespace KS.Scripting
namespace KS.Hardware
```

To better organize these namespaces, we decided to move them to more appropriate places outlined below:

```csharp
namespace KS.Shell.ShellBase.Commands.Execution
namespace KS.Kernel.Threading
namespace KS.ConsoleBase.Writers
namespace KS.Shell.ShellBase.Scripting
namespace KS.Kernel.Hardware
```

First, the `Execution` namespace had to do with executing processes, which was only being done by the shell and by the TMUX pane size detector. While it was referenced outside any shell code, we feel that it would be more appropriate to move it to the shell code.

Second, the kernel threading functionality got so mature that it was moved to the kernel code, showing that it really had great importance.

Third, all of the writers had to do with the console upon analyzing the namespace. As a result, we had to move them to the `ConsoleBase` namespace so that it reflects the purpose more appropriately namespace-wise.

As for the scripting, the only code which was calling it was the UESH shell command parser, which was part of the shell. For this reason, we decided to move the namespace to better show that it was part of the shell code.

FInally, for the hardware code, it went straight to the kernel namespace as it felt that it's really a part of the kernel. The hardware code gets called each time the kernel starts and as the `hwinfo` command gets executed.

{% hint style="info" %}
You'll have to adjust the namespaces as indicated in the second code block that shows the new namespaces for your mod to continue working.
{% endhint %}

### Moved JSON beautifier and minifier functions to `JsonTools`

{% code title="JsonBeautifier.cs and JsonMinifier.cs" lineNumbers="true" %}
```csharp
public static string BeautifyJson(string JsonFile)
public static string BeautifyJsonText(string JsonText)
public static string MinifyJson(string JsonFile)
public static string MinifyJsonText(string JsonText)
```
{% endcode %}

These functions used to exist in their own namespace with their own classes that are categorized based on the JSON formatting type on serialization. However, `JsonTools` looked like that it'd be the better place for them, so we've moved them into said class.

{% hint style="info" %}
To continue using these functions, you'll now have to change the using clause to point to the `KS.Misc.Editors.JsonShell` namespace and the class to `JsonTools`.
{% endhint %}

### Moved path lookup properties

{% code title="ShellManager.cs" lineNumbers="true" %}
```csharp
public readonly static string PathLookupDelimiter
public static string PathsToLookup
```
{% endcode %}

These path lookup properties used to exist in the shell management code, which indicated that these variables were exclusive for the shell command parser. However, at the same time, `PathLookupTools` existed, and we thought that it would be more appropriate to move these into that class, so we decided to move them into said class.

{% hint style="info" %}
`PathLookupDelimiter` has become a property, because it was included in the public API.

If you want to continue using these variables, we suggest you to change the class in all your references to them from `ShellManager` to `PathLookupTools`.
{% endhint %}

### Removed the field manager

{% code title="FieldManager.cs" lineNumbers="true" %}
```csharp
public static class FieldManager
```
{% endcode %}

When this was implemented, we relied on it for kernel configuration to get the variable values by name from the kernel configuration declaration file (known as SettingsEntries.json and its ancestors for screensaver and splash settings).

However, following several configuration system improvements that occurred two times:

* We've started working on the Reflection-based configuration reader and writer back at very early development stages by the following commits (in chronological order):
  * [a70787e](https://github.com/Aptivi/NitrocidKS/commit/a70787eafd44c32e7ab6ad7814c587dc03d62dc9): Implemented new config reader (experimental)
  * [9d350f4](https://github.com/Aptivi/NitrocidKS/commit/9d350f4b15be6bb2f01d346785800bd949e098ff): Added new config writer
  * [7d134f8](https://github.com/Aptivi/NitrocidKS/commit/7d134f853a516defb9c494c56f2c9f0f88bd8175): Finalized the new config writer
  * [94d1789](https://github.com/Aptivi/NitrocidKS/commit/94d1789f05f76589d9d7eedd7b344ef67c6f12b8): Removed the old config reader/writer
  * [ec3623f](https://github.com/Aptivi/NitrocidKS/commit/ec3623f332e74a67581c204d4791624af17aff86): Used expressions to try to speed up config read/write
* We've removed the Reflection-based configuration as it was deemed to be very slow, and replaced its implementation with the serialization-based approach. The first commit has extensive information about the two config system merges. The following commits were done in the chronological order:
  * [f6c1386](https://github.com/Aptivi/NitrocidKS/commit/f6c1386c0a93c946882ed989f8ac04436b18aab6): Merging to serialization-based config
  * [5f15a29](https://github.com/Aptivi/NitrocidKS/commit/5f15a29f602d2ac0c07087228c4a502e6f4d389f): Finalized Settings for serialization-based config (pt.1)
  * [e76cca2](https://github.com/Aptivi/NitrocidKS/commit/e76cca2ce18b5a79c9abf6553386e605039c7e69): Finalized Settings for serialization-based config (pt.2)

With regards to the last breaking change, which was done by commit [b96a724](https://github.com/Aptivi/NitrocidKS/commit/b96a7240156fb24993680f28a189b515a5fdb04d), the last "delimiter string variable" was changed from it being a field to it being a property, causing us to remove the entire field manager from the `Reflection` namespace.

{% hint style="danger" %}
We advice you to cease using this class and its functions, especially since it's useless for public fields that will either get converted to properties or get removed (if any).
{% endhint %}
