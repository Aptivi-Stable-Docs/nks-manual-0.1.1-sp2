---
description: Guide for upgrading 0.1.0 Beta 3 mods to 0.1.0 RC
---

# ⬆️ From 0.1.0 Beta 3 to 0.1.0 RC

This page lists all the changes that have been made from 0.1.0 Beta 3 to 0.1.0 RC. For upgrading your mods from 0.0.24.x directly to the 0.1.0 series, use the main upgrade page where it highlights the most important changes.

## From Beta 3 to RC

During RC's development, we have made the following breaking changes:

### Removed "mod part" leftovers

{% code title="IMod.cs" lineNumbers="true" %}
```csharp
/// <summary>
/// Name of part of mod
/// </summary>
string ModPart { get; set; }
```
{% endcode %}

When we removed the mod management code related to mod parts, there were leftovers to that deprecated feature, and none of the mod management parts seem to be using the mod parts feature after its removal on Beta 3.

So, we've decided to ease the task on the mod developers by removing the mod part leftovers, essentially removing the `ModPart` variable that you have to override.

{% hint style="danger" %}
You can no longer use this function as of 0.1.0 Release Candidate.
{% endhint %}

### Changed the root namespace

{% code title="All code files" lineNumbers="true" %}
```csharp
namespace KS.(...)
{
    (...)
}
```
{% endcode %}

The root namespace of NItrocid KS has been finally changed to `Nitrocid` from the abbreviation of the older name, Kernel Simulator (`KS`), to achieve consistency across the whole set of projects, especially Nitrocid's addons that use the newer `Nitrocid` namespace over the older one, `KS`.

As a result, all mods will break due to the change to the root namespace.

{% hint style="info" %}
The root namespace has been changed from KS to Nitrocid, so you need to update all your usings clause to point to the new root namespace. For example, if you're referring to `KS.ConsoleBase`, you need to update it to point to `Nitrocid.ConsoleBase`.
{% endhint %}

### Custom splashes folder removed

{% code title="PathsManagement.cs" lineNumbers="true" %}
```csharp
public static string CustomSplashesPath
```
{% endcode %}

To accommodate the usage of the mod importance, we've removed the custom splashes folder path from the kernel paths. This is because we've added the registration and the unregistration of the custom splashes, which further simplifies the complexity of the custom splashes.

This simplification is going to be pointed out in a separate breaking change section.

{% hint style="info" %}
Your custom splashes are now found in the `KSMods` folder instead of the `KSSplashes` folder.
{% endhint %}

### Changed the splashes model to (un)registration model

```csharp
// Modified
public static List<string> GetNamesOfSplashes()

// Removed
public static void LoadSplashes()
public static void UnloadSplashes()
public static ISplash GetSplashInstance(Assembly Assembly)
```

We've changed the splash loading form to use the registration and the unregistration model. This allows your splashes to be registered in the easiest method possible.

Furthermore, `GetNamesOfSplashes()` has been changed to return an array of strings containing the names of both the base and the custom splashes, making it read-only.

As a result, the removed APIs that are mentioned in the above code block have been removed to respect the changes made to the splash loading system.

{% hint style="info" %}
Your mod should set the importance priority (`LoadPriority`) to `Important` to be able to load the splashes early before configuration parsing takes place.
{% endhint %}

### Mandatory mod version and name properties

{% code title="IMod.cs" lineNumbers="true" %}
```csharp
string Name { get; set; }
string Version { get; set; }
```
{% endcode %}

Earlier, the mods can specify empty version and empty name so that they can have mod file name as the mod name. Now, we've made these properties mandatory so that they can no longer be optional.

Furthermore, we've changed the two properties to be read-only to avoid name and version manipulation.

{% hint style="info" %}
You need to specify a valid SemVer 2.0 compliant version for your mod and your mod name in order to get loaded.
{% endhint %}

### Merged text tools to Textify

This merger contains two changes: removals and migrations.

#### Removals

{% code title="Removed classes and enums" lineNumbers="true" %}
```csharp
public static class CharManager
public static class TextTools
public enum EnclosedDoubleQuotesType
```
{% endcode %}

We've removed these two classes to avoid duplicate code that already exists in Textify for consistency.

#### Migrations

{% code title="Minified classes" lineNumbers="true" %}
```csharp
public static class FigletTools
public static class JsonTools
```
{% endcode %}

We've also minified the two above classes to their absolute minimum to only include properties and functions that are specific to Nitrocid KS.

### Wrapped classes with Terminaux

Terminaux contained changes merged with the latest development branch of Nitrocid KS 0.1.0, so we've listed the following classes that got merged with Terminaux:

{% code title="Classes" lineNumbers="true" %}
```csharp
public class Screen
public class ScreenPart
public static class ScreenTools
public static class ColorSelector
public class InputChoiceInfo
public static class InputChoiceTools
public static class Input
public enum ChoiceOutputType
public static class ChoiceStyle
public static class InfoBoxButtonsColor
public static class InfoBoxColor
public static class InfoBoxInputColor
public static class InfoBoxProgressColor
public static class InfoBoxSelectionColor
public static class InfoBoxSelectionMultipleColor
public static class InfoBoxTitledButtonsColor
public static class InfoBoxTitledColor
public static class InfoBoxTitledInputColor
public static class InfoBoxTitledProgressColor
public static class InfoBoxTitledSelectionColor
public static class InfoBoxTitledSelectionMultipleColor
public static class InputStyle
public static class SelectionMultipleStyle
public static class SelectionStyle
public class BaseInteractiveTui : IInteractiveTui
public interface IInteractiveTui
public class InteractiveTuiBinding
public static class InteractiveTuiTools
public static class ListEntryWriterColor
public static class ListWriterColor
public static class TextWriterColor
public static class TextWriterSlowColor
public static class TextWriterWhereColor
public static class TextWriterWhereSlowColor
public static class TextWriterWrappedColor
public static class BorderColor
public static class BorderTextColor
public static class BoxColor
public static class BoxFrameColor
public static class BoxFrameTextColor
public static class CenteredFigletTextColor
public static class CenteredTextColor
public static class FigletColor
public static class FigletWhereColor
public static class PowerLineColor
public static class ProgressBarColor
public static class ProgressBarVerticalColor
public static class SeparatorWriterColor
public static class TableColor
public static class BorderTools
public class CellOptions
public class PowerLineSegment
public static class PowerLineTools
public static class ProgressTools
public static class LineHandleRangedWriter
public static class LineHandleWriter
public static class ConsoleChecker
public static class ConsoleResizeListener
public static class ConsoleResizeHandler
public static class ConsoleWrapper
public class ChoiceInputElement : IElement
public class DynamicTextElement : IElement
public interface IElement
public class InputElement : IElement
public class MaskedInputElement : IElement
public class MultipleChoiceInputElement : IElement
public class TextElement : IElement
public class PresentationPage
public static class PresentationTools
public class Slideshow
```
{% endcode %}

In addition to the classes, we've moved the following functions:

{% code title="KernelColorTools.cs" lineNumbers="true" %}
```csharp
public static void LoadBack()
public static void LoadBack(Color ColorSequence)
public static Color GetGray()
public static void SetConsoleColor(Color ColorSequence, bool Background = false)
public static bool TrySetConsoleColor(Color ColorSequence, bool Background)
public static bool TryParseColor(string ColorSpecifier)
public static bool TryParseColor(int ColorNum)
public static bool TryParseColor(int R, int G, int B)
public static Color GetRandomColor(bool selectBlack = true)
public static Color GetRandomColor(ColorType type, bool selectBlack = true)
public static Color GetRandomColor(ColorType type, int minColor, int maxColor, int minColorR, int maxColorR, int minColorG, int maxColorG, int minColorB, int maxColorB)
```
{% endcode %}

{% code title="ConsoleExtensions.cs" lineNumbers="true" %}
```csharp
public static void ClearKeepPosition()
public static string GetClearLineToRightSequence()
public static void ClearLineToRight()
public static int PercentRepeat(int CurrentNumber, int MaximumNumber, int WidthOffset)
public static int PercentRepeatTargeted(int CurrentNumber, int MaximumNumber, int TargetWidth)
public static string FilterVTSequences(string Text)
public static (int, int) GetFilteredPositions(string Text, bool line, params object[] Vars)
public static void SetTitle(string Text)
public static void ResetAll()
```
{% endcode %}

Terminaux provides all of these functions and classes. To reduce maintenance burden associated with the duplicated code, we've removed all the duplicated code in the Nitrocid KS codebase. As a result, we've moved all its associated documentation to Terminaux to also reduce the documentation maintenance burden.

{% hint style="info" %}
You can no longer call these functions directly from Nitrocid KS's `ConsoleBase` namespace. You must make use of Terminaux's namespaces instead.
{% endhint %}

### Separated driver config from main kernel config

{% code title="KernelMainConfig.cs" lineNumbers="true" %}
```csharp
public string Current***Driver
```
{% endcode %}

We've separated the driver configuration from the main kernel configuration so that we can separately manage the kernel drivers without having to modify the main kernel configuration. This allows such configuration to be much more portable from before.

{% hint style="info" %}
You need to update the references to the above properties found in `KernelMainConfig` to point to `DriverConfig` instead of `MainConfig`.
{% endhint %}

### Used double-precision floats for progress handling

{% code title="ProgressHandler.cs" lineNumbers="true" %}
```csharp
public Action<int, string> ProgressAction
public ProgressHandler(Action<int, string> progressAction, string context)
```
{% endcode %}

{% code title="ProgressManager.cs" lineNumbers="true" %}
```csharp
public static void ReportProgress(int progress, string context)
public static void ReportProgress(int progress, string context, string message)
public static void ReportProgress(int progress, string context, string message, params object[] vars)
```
{% endcode %}

To more accurately tell whether the progress has finished or not, we're introducing the double-precision floating-point numbers to the progress handler and the manager, effectively improving the progress handling for long tasks.

{% hint style="info" %}
There's no longer a need to cast a progress number to the integer when reporting progress; it already makes use of the double-precision floating-point numbers now.
{% endhint %}

### Moved MAL/MOTD parsers to `Login`

{% code title="MalParse.cs and MotdParse.cs" lineNumbers="true" %}
```csharp
namespace Nitrocid.Misc.Text.Probers.Motd
```
{% endcode %}

Earlier, the MOTD and the MAL parsers belonged to the miscellaneous portion of the kernel. However, according to our examination, only the login handler seems to be actively using it. This means that it's more appropriate for these parsers to move to the `Login` namespace.

{% hint style="info" %}
None of the classes have their functionality changed. You can just update the usings clause to point to `Nitrocid.Users.Login.Motd` instead of `Nitrocid.Misc.Text.Probers.Motd`.
{% endhint %}

### Removed three addons from the known addons list

{% code title="KnownAddons.cs" lineNumbers="true" %}
```csharp
public enum KnownAddons
{
    (...)
    /// <summary>
    /// ChatGPT Unofficial Client Extras addon
    /// </summary>
    ExtrasChatGpt,
    (...)
    /// <summary>
    /// RetroKS Extras addon
    /// </summary>
    ExtrasRetroKS,
    (...)
    /// <summary>
    /// Inxi.NET Hardware Prober Legacy addon
    /// </summary>
    LegacyInxiNet,
    (...)
}
```
{% endcode %}

As a result of the three addons being removed at the start of the release candidate development cycle, we've decided to remove their leftovers: code files and known addons list. This is to ensure that the inter-addon communication doesn't expect any more deleted addons.

{% hint style="danger" %}
You can no longer use these addons to perform inter-addon communication.
{% endhint %}

### Command lists, revamped

{% code title="IShellInfo.cs" lineNumbers="true" %}
```csharp
Dictionary<string, CommandInfo> Commands { get; }
Dictionary<string, CommandInfo> ModCommands { get; }
```
{% endcode %}

The two properties used to host a single dictionary that contained a key for the command name and a value for the actual command information instance. Over time, we've discovered that specifying a command two times when building the shell info is redundant, so we've decided to convert the two properties to a list to make the job easier for the mod developers to assign their own command and to reduce confusion.

However, the following properties had to be modified during the change:

{% code title="ShellManager.cs" lineNumbers="true" %}
```csharp
public static ReadOnlyDictionary<string, CommandInfo> UnifiedCommands
```
{% endcode %}

In addition to that, the following functions had to be modified:

{% code title="ModManager.cs" lineNumbers="true" %}
```csharp
public static Dictionary<string, CommandInfo> ListModCommands(ShellType ShellType)
public static Dictionary<string, CommandInfo> ListModCommands(string ShellType)
```
{% endcode %}

{% code title="CommandManager.cs" lineNumbers="true" %}
```csharp
public static bool IsCommandFound(string Command, ShellType ShellType, bool includeAliases = true)
public static bool IsCommandFound(string Command, string ShellType, bool includeAliases = true)
public static bool IsCommandFound(string Command, bool includeAliases = true)
public static Dictionary<string, CommandInfo> GetCommands(ShellType ShellType, bool includeAliases = true)
public static Dictionary<string, CommandInfo> GetCommands(string ShellType, bool includeAliases = true)
public static Dictionary<string, CommandInfo> FindCommands([StringSyntax(StringSyntaxAttribute.Regex)] string namePattern, ShellType ShellType, bool includeAliases = true)
public static Dictionary<string, CommandInfo> FindCommands([StringSyntax(StringSyntaxAttribute.Regex)] string namePattern, string ShellType, bool includeAliases = true)
public static CommandInfo GetCommand(string Command, ShellType ShellType, bool includeAliases = true)
public static CommandInfo GetCommand(string Command, string ShellType, bool includeAliases = true)
```
{% endcode %}

As a result, the alias management had to change a bit on how it worked to adjust with this kind of change, resulting in a property in `CommandInfo`, called `Aliases`, showing up.

The reason for this revamp is that because we needed to reduce the complexity of defining commands, especially when it comes to command names, you had to write it twice: the first time on the key and the second time on the `CommandInfo` constructor.

{% hint style="info" %}
You'll have to do the following:

* For the functions, the `includeAliases` argument is gone. As a result, you'll have to cut the last argument passed to it.
* `ListModCommands()` now returns an array of `CommandInfo` instead of a dictionary.
*   You need to update your public overrides as illustrated in the below code block:

    ```diff
    -public override Dictionary<string, CommandInfo> Commands => new() ...
    +public override List<CommandInfo> Commands => new() ...
    ```
{% endhint %}

### Moved network classes one level shallow

{% code title="Network classes" lineNumbers="true" %}
```csharp
namespace Nitrocid.Network.Base.Connections
namespace Nitrocid.Network.Base.SpeedDial
namespace Nitrocid.Network.Base.Transfer
namespace Nitrocid.Network.RPC
namespace Nitrocid.Network.RSS
```
{% endcode %}

The network namespace needed better organization, because a lot of network-based shells were moved to their own addons. As a result, we've moved all the classes under `Base` one level closer to the root namespace, while the RPC and the RSS classes were moved to the `Types` namespace.

{% hint style="info" %}
This movement is done based on the fact that the network classes needed better organization. You'll have to update the following namespaces:

```csharp
namespace Nitrocid.Network.Connections
namespace Nitrocid.Network.SpeedDial
namespace Nitrocid.Network.Transfer
namespace Nitrocid.Network.Types.RPC
namespace Nitrocid.Network.Types.RSS
```
{% endhint %}

### Added file combination to `Manipulation`

{% code title="Combination.cs" lineNumbers="true" %}
```csharp
public static class Combination
```
{% endcode %}

The file combination methods from the above class were considered to be file manipulation functions, so we've moved these functions to the `Manipulation` class:

* `CombineTextFiles()`
* `CombineBinaryFiles()`

In addition to that, we've removed the `Combination` class as it only contained the two above functions.

{% hint style="info" %}
Change the references of `Combination` to `Manipulation` class when calling these functions.
{% endhint %}
