---
description: Follow the compatibility notes when upgrading your mods to API v1.3 series
---

# 🔼 Upgrading to API v1.3 series

When upgrading your modification from the target of the later version of Nitrocid KS that declares itself to be from the API v1.3, you must make necessary changes to be able to use your mod in a Nitrocid KS version which you use to test your mod. These changes will be listed starting from 0.0.16 to the last version in this API revision.

## From 0.0.15 to 0.0.16

This version refined the kernel in various aspects, including the enhancements of the coloring system and the addition of various useful features.

{% content-ref url="../version-release-notes/v0.0.16.x-series.md" %}
[v0.0.16.x-series.md](../version-release-notes/v0.0.16.x-series.md)
{% endcontent-ref %}

### `TemplateSet()` and `ParseCurrentTheme()`

{% code title="Color.vb" lineNumbers="true" %}
```visual-basic
Public Sub TemplateSet(ByVal theme As String)
Public Sub ParseCurrentTheme()
```
{% endcode %}

One of the useful features added to this version series of Nitrocid KS is the dynamic theme support, which takes any theme information formatted in JSON format and parses it to the theme manager to set the necessary colors according to the color types.

This function was implemented to set the kernel colors from any theme name. However, the major caveat that affected this function was that themes depended on the supporting variables for each theme, which ultimately resulted in the API pollution that we had to clean up. Refining the theme feature caused us to remove this function and replace it with something saner.

{% hint style="info" %}
If you want to apply your theme from either the pre-defined kernel themes or the custom theme file, use one of the three functions. Their method signatures are shown below.

The second function, `ParseCurrentTheme()`, was actually checking the kernel colors against all the themes that were defined and, if matched, returned the appropriate theme name. Such function is not available as an alternative in the current version of the kernel and may not arrive soon.

{% code title="ThemeTools.cs" lineNumbers="true" %}
```csharp
public static void ApplyThemeFromResources(string theme)
public static void ApplyThemeFromFile(string ThemeFile)
public static void SetColorsTheme(ThemeInfo ThemeInfo)
```
{% endcode %}
{% endhint %}

### `ListDrivers()`

{% code title="HardwareProbe.vb" lineNumbers="true" %}
```visual-basic
Public Sub ListDrivers()
```
{% endcode %}

This function listed all the parsed hardware that Inxi.NET parsed. Since 0.0.16, a better alternative to this function, `ListHardware()`, surfaced, causing this function to be removed from the public API.

{% hint style="info" %}
`ListHardware()` offers the same functionality as this function, except that it now takes a specific hardware type or all the types. To upgrade your mod to the latest API, you need to use the below function.

{% code title="HardwareList.cs" lineNumbers="true" %}
```csharp
public static void ListHardware(string HardwareType)
```
{% endcode %}
{% endhint %}

### `NotEnoughArgumentsException` class

{% code title="EventsAndExceptions.vb" lineNumbers="true" %}
```visual-basic
Public Class NotEnoughArgumentsException
```
{% endcode %}

Due to how the shell worked in API 1.2 or earlier, this exception was implemented to try to tell the user that they didn't provide enough arguments for each command that required arguments to be provided. As the recent shell breakthroughs happened, we've removed the entire exception to replace it with a faster alternative.

{% hint style="danger" %}
It's clunky to throw this exception each time insufficient arguments were provided, so we advice you to cease using this function.
{% endhint %}

### Synth system

{% code title="BeepSynth.vb" lineNumbers="true" %}
```visual-basic
Public Function ProbeSynth(ByVal file As String) As Boolean
```
{% endcode %}

{% code title="EventsAndExceptions.vb" lineNumbers="true" %}
```visual-basic
Public Class InvalidSynthException
```
{% endcode %}

Beep synth system was implemented to create your PC speaker songs using only the frequencies in hertz and lengths in milliseconds. It was removed because it was later realized that the beep method used wasn't cross-platform according to the .NET 6.0 manual as described in the below link:

{% embed url="https://learn.microsoft.com/en-us/dotnet/api/system.console.beep?view=net-7.0#system-console-beep(system-int32-system-int32)" %}

As a result, the entire synth system was removed.

{% hint style="danger" %}
This function was deemed not to work on systems that aren't using Windows. If you want to continue using the synth system, compile the mod example that holds the source code of the system from source. If you want your mod to work in Linux, either implement your own wrapper to the beep system for your Linux kernel version, or stop using this function and start implementing visual alerts.
{% endhint %}

### `GetCultureFromLang()`

{% code title="Translate.vb" lineNumbers="true" %}
```visual-basic
Public Function GetCultureFromLang() As String
```
{% endcode %}

This method used to return culture names from the current kernel language. However, it was later found to be using hard-coded culture names because a lot of maintenance affected it regarding names that worked in both Windows and Linux, causing it to be later vanished.

{% hint style="info" %}
The same functionality made a return, but, this time, we used variables.

{% code title="CultureManager.cs" lineNumbers="true" %}
```csharp
public static string CurrentCultStr
public static CultureInfo CurrentCult
public static List<CultureInfo> GetCulturesFromLang(string Language)
public static List<string> GetCultureNamesFromCurrentLang()
public static List<string> GetCultureNamesFromLang(string Language)
```
{% endcode %}
{% endhint %}

### Configuration Renovation

We had three types of different configuration renovation that caused API removals during the development period of 0.0.16.

#### User configuration renovation

{% code title="UserManagement.vb" lineNumbers="true" %}
```visual-basic
Function GetUserEncryptedPassword(ByVal User As String)
```
{% endcode %}

This function first appeared at the start of API v1.2 as a private API function to get  encrypted user password.

#### Alias configuration renovation

{% code title="AliasManager.vb" lineNumbers="true" %}
```visual-basic
Public Sub CloseAliasesFile()
```
{% endcode %}

It was used as a shortcut to close the old-style alias list file.

#### Central configuration renovation

{% code title="Config.vb" lineNumbers="true" %}
```visual-basic
Public Function CheckForUpgrade() As Boolean
Public Sub UpdateConfig()
```
{% endcode %}

{% code title="OldConfigUp.vb" lineNumbers="true" %}
```visual-basic
Sub UpgradeConfig()
```
{% endcode %}

The functions that were found in the Config module were used solely to check the existing kernel configuration file for updates and rebuilds the configuration file to make upgrades between kernel versions go smoother.

The last routine, however, converts the kernel configuration file created by API v1.0 kernels to the usable form.

{% hint style="danger" %}
All of the functions were removed as a result of the deprecation of the converter tool that last shipped with API v2.1 kernels.

Furthermore, as of 0.1.0 Beta 1, the configuration was improved to the point that it witnessed performance improvements.
{% endhint %}

### True color writers

{% code lineNumbers="true" %}
```visual-basic
'Normal
Public Sub WriteTrueColor(ByVal text As Object, ByVal Line As Boolean, ByVal ColorRGBFG As RGB, ByVal ColorRGBBG As RGB, ByVal ParamArray vars() As Object)
Public Sub WriteTrueColor(ByVal text As Object, ByVal Line As Boolean, ByVal ColorRGB As RGB, ByVal ParamArray vars() As Object)

'Slow
Public Sub WriteSlowlyTrueColor(ByVal msg As String, ByVal Line As Boolean, ByVal MsEachLetter As Double, ByVal ColorRGB As RGB, ParamArray ByVal vars() As Object)
Public Sub WriteSlowlyTrueColor(ByVal msg As String, ByVal Line As Boolean, ByVal MsEachLetter As Double, ByVal ColorRGBFG As RGB, ByVal ColorRGBBG As RGB, ParamArray ByVal vars() As Object)

'Positional
Public Sub WriteWhereTrueColor(ByVal msg As String, ByVal Left As Integer, ByVal Top As Integer, ByVal ColorRGBFG As RGB, ByVal ColorRGBBG As RGB, ByVal ParamArray vars() As Object)
Public Sub WriteWhereTrueColor(ByVal msg As String, ByVal Left As Integer, ByVal Top As Integer, ByVal ColorRGB As RGB, ByVal ParamArray vars() As Object)
```
{% endcode %}

These functions used the ancient RGB class implemented by the kernel itself to specify the true RGB values of any color we want. However, as the Color class came (now in ColorSeq), this caused us to merge these functions to the Color class.

{% hint style="info" %}
You can still use `Write()` and their siblings using the Color class.
{% endhint %}

### `ParseMods()`

{% code title="ModParser.vb" lineNumbers="true" %}
```visual-basic
Sub ParseMods(ByVal StartStop As Boolean)
```
{% endcode %}

This function was used as an on/off lever to trigger all mods in one go. It got split to two separate functions, `StartMods()` and `StopMods()`.

{% hint style="info" %}
You can still use the two separate functions in the latest kernel version series. But, be careful if you're using these APIs! You could be stopping your own mods! Their method signatures are shown below.

{% code title="ModManager.cs" lineNumbers="true" %}
```csharp
public static void StartMods()
public static void StopMods()
public static void StartMod(string ModFilename)
public static void StopMod(string ModFilename)
```
{% endcode %}
{% endhint %}

### `SFTPPromptForPassword()`

{% code title="SFTPTools.vb" lineNumbers="true" %}
```visual-basic
Public Sub SFTPPromptForPassword(ByVal user As String, ByVal Address As String, ByVal Port As Integer)
```
{% endcode %}

This function was used to prompt the user for the SFTP password for the specified address. As `GetConnectionInfo()` got implemented, we've removed this function.

{% hint style="danger" %}
We advice you to cease using this function.
{% endhint %}

## From 0.0.16 to 0.0.17

This version was released as a small update to add minor features and general improvements.

{% content-ref url="../version-release-notes/v0.0.17.x-series.md" %}
[v0.0.17.x-series.md](../version-release-notes/v0.0.17.x-series.md)
{% endcontent-ref %}

### `InitTimesInZones()`

{% code title="TimeZones.vb" lineNumbers="true" %}
```visual-basic
Public Sub InitTimesInZones()
```
{% endcode %}

This function used to populate all the available time zones from your computer to cache it to the public field containing timezones. However, it was later removed for said reason.

{% hint style="danger" %}
We advice you to cease using this function.
{% endhint %}

### `ShowTimesInZones()`

{% code title="TimeZones.vb" lineNumbers="true" %}
```visual-basic
Public Sub ShowTimesInZones(Optional ByVal zone As String = "all")
```
{% endcode %}

This routine was used to show information about all or selected time zone, including the UTC offsets. This function was later split to three separate functions.

{% hint style="info" %}
To use this function in the latest API revision, use these three functions listed in the below code block.

Beware that you need to have **`tzdata`** package installed on your Linux system!

{% code title="TimeZones.cs" lineNumbers="true" %}
```csharp
public static bool ShowTimeZone(string Zone)
public static bool ShowTimeZones(string Zone)
public static void ShowAllTimeZones()
```
{% endcode %}
{% endhint %}

## From 0.0.17 to 0.0.18

This version added some more features to the kernel, including the custom startup banner support.

{% content-ref url="../version-release-notes/v0.0.18.x-series.md" %}
[v0.0.18.x-series.md](../version-release-notes/v0.0.18.x-series.md)
{% endcontent-ref %}

### All help functions

{% code lineNumbers="true" %}
```visual-basic
Public Sub InitHelp()
Sub InitTShell()
Public Sub InitSFTPHelp()
Public Sub InitRDebugHelp()
Public Sub IMAPInitHelp()
Public Sub InitFTPHelp()
Public Sub ZipShell_UpdateHelp()
Public Sub TextEdit_UpdateHelp()
```
{% endcode %}

These functions were used to populate help information for all shells. However, their functions have been moved to the help list field initializers, marking these routines as null and void.

{% hint style="danger" %}
These functions were never meant to be available to the public API in the first place. We advice you to cease using this function. The enhanced help system introduced in recent API series adapts to such changes.
{% endhint %}
