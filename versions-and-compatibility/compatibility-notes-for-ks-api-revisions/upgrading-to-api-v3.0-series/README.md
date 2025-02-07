---
description: Follow the compatibility notes when upgrading your mods to API v3.0 series
---

# 🔼 Upgrading to API v3.0 series

As API v3.0 is still in development, the breaking changes get committed and land here. They describe what broke and what should be used instead.

{% hint style="danger" %}
## Critically important notices

When upgrading your mods to API v3.0, consider these:

* Starting from 0.1.0, old configuration styles are no longer supported and are kept in your home directory for reference. The kernel will start from scratch as if you've started it for the first time. Don't worry; all your configuration from 0.0.24.0 are still saved in your home directory. This is because of configuration system changes that happened between 0.0.24.0 and 0.1.0.
* .NET Framework 4.8 is **no longer supported** as of 0.1.0 Beta 1. Please consider using .NET 8.0 instead to continue using Nitrocid KS. This is to improve multi-platform support without having to go to one environment (`dotnetfx` on Windows and `mono` on Linux and macOS) for each platform. This is done starting from API version `3.0.25.310`.
* Starting from API version `3.0.25.123`, the mod versions should satisfy the SemVer v2.0 specification. Mod parsing will fail if an invalid version expression is entered.
* Starting from API version `3.0.25.154`, the mod loader will fail if the mod is not strongly signed using any strong naming key and loading untrusted mods is disabled.
* Starting from API version `3.0.25.361`, mods are no longer organized with mod parts. This feature has been removed and won't be coming back.
* Starting from API version `3.0.25.380`, the root namespace has been changed from KS to Nitrocid. This means that you need to update your mods to point to the newer root namespace.
* Starting from API version, `3.0.25.383`, the mod name and the mod version must be specified, because if they aren't specified, the mod parsing fails.

The explanation about these changes can be found at the bottom of this page.
{% endhint %}

## From 0.0.24 to 0.1.0

This version is a futuristic magic that brings in many feature additions and spectacular improvements in all fields, including cosmetic changes.

{% content-ref url="../../version-release-notes/v0.1.x.x-series.md" %}
[v0.1.x.x-series.md](../../version-release-notes/v0.1.x.x-series.md)
{% endcontent-ref %}

### Detailed important changes

This section explains how to adapt the important changes to your mod code so that it works with 0.1.0 and higher. This highlights the most important changes that we have compiled for you.

#### Old configuration styles

The following configuration files (incomplete list) have been changed entirely from 0.0.24.x to at least 0.1.0 to better fit the requirements of having the clean home directory (i.e. moved from the user profile directory to the local application data directory):

```
d-----          3/5/2024  12:20 PM                KSEvents
d-----          3/5/2024  12:20 PM                KSMods
d-----          3/5/2024  12:20 PM                KSReminders
d-----          3/5/2024  12:20 PM                KSSplashes
-a----          3/5/2024  12:20 PM              0 Aliases.json
-a----          3/5/2024  12:20 PM              0 DebugDeviceNames.json
-a----          3/5/2024  12:20 PM          31318 KernelConfig.json
-a----          3/5/2024  12:20 PM             32 MAL.txt
-a----          3/5/2024  12:20 PM             20 MOTD.txt
-a----          3/5/2024  12:20 PM            176 Users.json
```

After you upgrade Nitrocid KS 0.0.24.x to 0.1.0, running it may yield first-time run wizards, thinking that all your configuration is lost. However, they're not lost, because the old configuration styles will not be deleted by 0.1.0. Unfortunately, the two configuration styles are not compatible with each other, meaning that you'll have to manually replicate the configuration that you had made when you were using 0.0.24.x.

This is intentionally done to make the configuration reader and the writer faster than before due to the usage of the serialization techniques, compared to the old method of manually parsing every single configuration key and installing them to the appropriate variables. The new reader and writer also allows more flexibility due to how they're implemented, causing your mods to be more configurable than before.

{% hint style="info" %}
You can learn more about why we've made these changes, as well as a list of commits that may explain more, [here](from-0.1.0-beta-1-to-0.1.0-beta-2.md#removed-the-field-manager).
{% endhint %}

#### Removal of .NET Framework 4.8 support

After 0.0.24.x, you'll notice that we no longer ship a version that is compatible with the .NET Framework 4.8 that is pre-installed in the latest versions of Windows. This is because we have started using features that are exclusive to the modern .NET, and Nitrocid KS 0.1.0 and above currently uses .NET 8.0 as the minimum version needed to run. As such, we've decided to change the requirements according to this change.

{% hint style="info" %}
Please consider using .NET 8.0 instead to continue using Nitrocid KS. This is to improve multi-platform support without having to go to one environment (`dotnetfx` on Windows and `mono` on Linux and macOS) for each platform.
{% endhint %}

Fortunately, we still maintain the 0.0.24.x version, which still supports .NET Framework 4.8. This version will be supported until the full .NET Framework 4.8 deprecation, which will not happen until further notice.

#### Significant improvements to your mods

Starting from 0.1.0, we've significantly made improvements as to how your mods start. Your mods have become more powerful than before, and we're still working on it to ensure that they get more control over how Nitrocid and its addons work. As part of this significant improvement effort that happened during the entire development cycle of this version, we had to do the following important changes to ensure that your mods have the highest quality possible:

* We have removed organizing multiple mods that have the same name using mod parts, because they were relevant back when mods consisted of just a single source code file (Visual Basic or C#) and large mods would be grouped using multiple source code files that inter-operate with each other. Some versions ago, we had removed source-code-based mods and relied on mod developers to make `.DLL` versions to ensure that they get the most flexibility possible. This feature won't be coming back because of its rigidity. See why [here](from-0.1.0-beta-2-to-0.1.0-beta-3.md#removed-mod-parts).
* All pre-API v3.0 versions of Nitrocid KS, including 0.0.24.x, had `KS` as its root namespace, because the name was Kernel Simulator back then. We have given this application a name, called Nitrocid KS, and the kernel name has been changed to Nitrocid Kernel. As a result, the root namespace has been changed from `KS` to `Nitrocid` to be more consistent with the application branding. Some of the breaking change notes still use the `KS` root namespace for historical reasons and they should not be modified as they list the history of changes that happened between the previous version and the next version. You'll have to update your imports to point to the new root namespace. See why [here](from-0.1.0-beta-3-to-0.1.0-rc.md#changed-the-root-namespace).
* Earlier, we had allowed empty mod names and versions, because, back then, they were relevant in cases which the mod authors have decided not to provide the mod name and the mod version, but still provide the mod name as the file name of the loose source code file. Some versions ago, we had removed source-code-based mods for the first reason mentioned above. As a result, we've mandated the mod name and the mod version so that every mod that don't have their name and/or their version will not be parsed. They'll fail to load. See why [here](from-0.1.0-beta-3-to-0.1.0-rc.md#mandatory-mod-version-and-name-properties).
* Also, we have mandated the mod versioning via the SemVer v2.0 versioning scheme. This is to filter versions that are completely or partially invalid to ensure that only valid versions (revisions or not) are allowed. This is mandatory for mods that target Nitrocid KS 0.1.0 to ensure that the mod authors version their mods properly as all the kernel mods are glorified class libraries.
* Finally, we have made signing your mods using your strong name key mandatory. This is to make sure that your mods have a unique identity and that they are attributed to you. This is also to protect the kernel from rogue and often unsigned kernel mods that may cause problems with the kernel or your computer. Although strong naming is generally just for identity, Nitrocid's mod manager will refuse to load mods that are not strongly named for security.

For instructions on how to strongly name your mods, you can consult the below page to perform this operation.

{% content-ref url="../../../advanced-and-power-users/inner-workings/inner-essentials/assembly-signing.md" %}
[assembly-signing.md](../../../advanced-and-power-users/inner-workings/inner-essentials/assembly-signing.md)
{% endcontent-ref %}

### Other breaking changes

We have highlighted the most important breaking changes for your kernel mods. Now, for the other breaking changes, you'll need to adapt the changed from 0.0.24.x to 0.1.0 using the guides that are explained in the below pages:

{% content-ref url="from-0.0.24.x-to-0.1.0-beta-1.md" %}
[from-0.0.24.x-to-0.1.0-beta-1.md](from-0.0.24.x-to-0.1.0-beta-1.md)
{% endcontent-ref %}

{% content-ref url="from-0.1.0-beta-1-to-0.1.0-beta-2.md" %}
[from-0.1.0-beta-1-to-0.1.0-beta-2.md](from-0.1.0-beta-1-to-0.1.0-beta-2.md)
{% endcontent-ref %}

{% content-ref url="from-0.1.0-beta-2-to-0.1.0-beta-3.md" %}
[from-0.1.0-beta-2-to-0.1.0-beta-3.md](from-0.1.0-beta-2-to-0.1.0-beta-3.md)
{% endcontent-ref %}

{% content-ref url="from-0.1.0-beta-3-to-0.1.0-rc.md" %}
[from-0.1.0-beta-3-to-0.1.0-rc.md](from-0.1.0-beta-3-to-0.1.0-rc.md)
{% endcontent-ref %}

{% content-ref url="from-0.1.0-rc-to-0.1.0-final.md" %}
[from-0.1.0-rc-to-0.1.0-final.md](from-0.1.0-rc-to-0.1.0-final.md)
{% endcontent-ref %}

The below sections specify the post-0.1.0 releases.

## From 0.1.0.0 to 0.1.0.3

Between 0.1.0.0 and 0.1.0.3, we've made the following breaking changes:

### Updated Terminaux to 3.3.0.1

We've updated Terminaux to 3.3.0.1 to fix a bug related to the selection style crashing when pressing ESC. In consequence, because the fix was released after the 3.3.0 release, we had to increase the mod API version to `3.0.25.439`.

You can consult the list of breaking changes that result from upgrading Terminaux 3.2.0 to 3.3.0 by pressing the below button:

{% content-ref url="https://app.gitbook.com/s/G0KrE9Uk2AiblqjWtpAo/breaking-changes/api-v3.0" %}
[API v3.0](https://app.gitbook.com/s/G0KrE9Uk2AiblqjWtpAo/breaking-changes/api-v3.0)
{% endcontent-ref %}

## From 0.1.0 to 0.1.1

This version is another futuristic magic that brings in feature additions and spectacular improvements. This version is built on top of the 0.1.0 codebase to provide you with the most gorgeous features ever introduced.

{% content-ref url="../../version-release-notes/v0.1.x.x-series.md" %}
[v0.1.x.x-series.md](../../version-release-notes/v0.1.x.x-series.md)
{% endcontent-ref %}

### Detailed important changes

This section explains how to adapt the important changes to your mod code so that it works with 0.1.1 and higher. This highlights the most important changes that we have compiled for you.

#### Usage of Terminaux 4.x and VisualCard 1.x

Nitrocid has been updated to use Terminaux 4.x and VisualCard 1.x. This is to bring in tons of amazing new features from both the libraries that aim to provide you with the best user and developer experience. You'll need to consult their own manual page for more information about how to adapt your Nitrocid mods to align with the latest breaking changes. VisualCard's breaking changes page is at the top button, and Terminaux's breaking changes page is at the bottom button.

{% content-ref url="https://app.gitbook.com/s/bEvVwD4FK7bX7p8XtIPH/breaking-changes" %}
[Breaking Changes](https://app.gitbook.com/s/bEvVwD4FK7bX7p8XtIPH/breaking-changes)
{% endcontent-ref %}

{% content-ref url="https://app.gitbook.com/s/G0KrE9Uk2AiblqjWtpAo/breaking-changes" %}
[Breaking changes](https://app.gitbook.com/s/G0KrE9Uk2AiblqjWtpAo/breaking-changes)
{% endcontent-ref %}

#### Color requirement functions condensed

{% code title="ThemeTools.cs" lineNumbers="true" %}
```csharp
public static bool Is255ColorsRequired(string theme)
public static bool Is255ColorsRequired(ThemeInfo theme)
public static bool Is255ColorsRequired(Dictionary<KernelColorType, Color> colors)
public static bool IsTrueColorRequired(string theme)
public static bool IsTrueColorRequired(ThemeInfo theme)
public static bool IsTrueColorRequired(Dictionary<KernelColorType, Color> colors)
```
{% endcode %}

The color requirement functions have been condensed so that it becomes easier to use and to understand. They have been condensed into just one function: `MinimumTypeRequired()`.

{% hint style="info" %}
The `MinimumTypeRequired()` function has been implemented to simplify the color requirement detection functions. The signatures are basically the same, but you need to also provide the color type that you want to check at the end of the function.
{% endhint %}

#### `AlarmEntry` implementation

{% code title="AlarmTools.cs" lineNumbers="true" %}
```csharp
public static KeyValuePair<(string, string), DateTime> GetAlarmFromId(string alarmId)
```
{% endcode %}

To more easily manage the alarm instances, we had to implement the `AlarmEntry` class that contains all the necessary properties for each alarm that your mods can control. In order to implement this class, we had to edit all the dictionaries that represent a list of alarms by name so that we hold just the alarm entry instance.

{% hint style="info" %}
Most of the time, you won't need to make any changes to how you call the functions. You just need to be aware of the fact that `GetAlarmFromId()` now returns a `KeyValuePair<string, AlarmEntry>`.
{% endhint %}

#### Removed references to GRILO

{% code title="KernelPlatform.cs" lineNumbers="true" %}
```csharp
public static bool IsRunningFromGrilo()
```
{% endcode %}

As GRILO is being sunset due to the implementation of the bootloader and the kernel environment management tools to Nitrocid, we've decided to remove everything related to GRILO by removing all the functions that have connections with that application.

{% hint style="danger" %}
We advise you to cease using this function. Your bootable apps now need to reference Nitrocid and become a kernel mod in order to work again as a bootable app.
{% endhint %}

#### Access to the part options

{% code title="CommandArgumentPart.cs" lineNumbers="true" %}
```csharp
public Func<string[], string[]> AutoCompleter { get; private set; }
public bool IsNumeric { get; private set; }
public string[] ExactWording { get; private set; }
```
{% endcode %}

We needed to add support for argument descriptions, which would show up in the command-specific help entry for more clarification. However, this added to the burden of maintaining a larg amount of arguments in the `CommandArgumentPart` constructor. As a result, we had to replace these properties with a single property, `Options`, that grants you easy access to the command argument part options.

{% hint style="info" %}
The constructors are not affected; one of it has been provided an extra optional argument that specifies the unlocalized argument description.
{% endhint %}

#### Base screensaver configuration implementation changed

{% code title="MatrixBleed.cs" lineNumbers="true" %}
```csharp
public static class MatrixBleedSettings
```
{% endcode %}

When the Nitrocid KS settings implementation still relied on manual key access and value casting, such as the usage of the indexer that accessed the settings section and keys, we had to proxy the screensaver settings due to the fact that we didn't have a concrete way of accessing the screensaver settings in a meaningful way.

Historically, going back to 0.0.12.0, it was the first kernel series that has added screensaver configuration in its minimal form, such as allowing true color and 256 color modes. Since then, the release of 0.0.20.0 redefined the configuration in a way that became necessary for us to change the settings handler. We used to proxy the screensaver settings because we were manually creating settings entries and processing them. This proxying came in two forms:

* The first list of an individual screensaver settings list contained only value setting, and this is found in the main configuration instance.
* The second list of an individual screensaver settings list contained value setting properties and value checks as implemented in the same file that implemented that screensaver.

For consistency and ease of maintenance, we've decided to remove this proxying, causing all the value check code to move to the kernel screensaver settings instance.

{% hint style="info" %}
Use the screensaver settings instance directly using the `SaverConfig` property in the `Config` class.
{% endhint %}

#### VSIX analyzers no longer shipped

We're no longer shipping the VSIX version of the Nitrocid analyzers because of low interest. Additionally, this version of the analyzers only worked in the full Visual Studio installation, which is only applicable for Windows systems. Moreover, the Community version of Visual Studio and all the other .NET IDEs, such as Rider, can't use this extension, and will have to rely on the NuGet package for analyzers.

Starting from 0.1.1, we've switched the distribution from VSIX to NuGet to be able to selectively install the analyzer to your project of your choice.

{% hint style="warning" %}
You are required to remove the VSIX version to be able to use the NuGet version of the analyzers, because they conflict with each other.
{% endhint %}

#### Moved JSON tools to Textify

{% code title="JsonTextTools.cs" lineNumbers="true" %}
```csharp
public static class JsonTextTools
```
{% endcode %}

We've moved this class to Textify and made sure that all of the functions that use this class point to the newer `JsonTools` class that Textify provides. This is because we wanted these functions to be independent of Nitrocid so that only grabbing Textify would be enough.

{% hint style="info" %}
Change the function references to point to `JsonTools` instead of `JsonTextTools`.
{% endhint %}

#### Removed a RetroKS path property

{% code title="PathsManagement.cs" lineNumbers="true" %}
```csharp
public static string RetroKSDownloadPath
```
{% endcode %}

We've removed a leftover that was spotted during the analysis of the paths during implementation of the widgets. This property was not removed in the final version of Nitrocid KS 0.1.0, but all the code that refers to the RetroKS project was removed due to the obsolescence.

{% hint style="danger" %}
We advice you to stop using this property.
{% endhint %}

#### Centered writer breaking changes

{% code title="TextFancyWriters.cs" lineNumbers="true" %}
```csharp
public static void WriteCentered(int top, string Text, KernelColorType ColTypes, params object[] Vars)
public static void WriteCentered(int top, string Text, KernelColorType colorTypeForeground, KernelColorType colorTypeBackground, params object[] Vars)
public static void WriteCentered(string Text, KernelColorType ColTypes, params object[] Vars)
public static void WriteCentered(string Text, KernelColorType colorTypeForeground, KernelColorType colorTypeBackground, params object[] Vars)
public static void WriteCenteredOneLine(int top, string Text, KernelColorType ColTypes, params object[] Vars)
public static void WriteCenteredOneLine(int top, string Text, KernelColorType colorTypeForeground, KernelColorType colorTypeBackground, params object[] Vars)
public static void WriteCenteredOneLine(string Text, KernelColorType ColTypes, params object[] Vars)
public static void WriteCenteredOneLine(string Text, KernelColorType colorTypeForeground, KernelColorType colorTypeBackground, params object[] Vars)
```
{% endcode %}

Terminaux 4.2.0 and later have introduced a breaking change that involves how the centered text writers work in terms of passing the parameters to the generator. As part of the widget system introduction, we needed to add the margin system. As a result, you'll notice that all calls to this function with the `Vars` argument will need to be revised.

{% hint style="info" %}
You'll need to rewrite how you call these functions so that you either pass a single object array like this: `Vars: vars`, or specify the margin values. You can always default to zeroes, and then you can pass the variables as if nothing happened.
{% endhint %}

#### Symmetric and asymmetric encoding drivers separated

{% code title="IEncodingDriver.cs" lineNumbers="true" %}
```csharp
bool IsSymmetric { get; }
```
{% endcode %}

{% code title="BaseEncodingDriver.cs" lineNumbers="true" %}
```csharp
public virtual bool IsSymmetric =>
    true;
```
{% endcode %}

In order to make implementation of the encoding drivers easier than before, we had to separate the encoding driver to make two versions: Symmetric and asymmetric. We've preserved the name of the symmetric version, while we've given the asymmetric version a new name: `EncodingAsymmetric`.

{% hint style="info" %}
You should change the driver type to match the symmetry type of your encoding algorithm driver. Either way, you should remove the `IsSymmetric` override. If your driver type is:

* symmetric, you don't need to do anything other than removing the `IsSymmetric` override.
* asymmetric, you need to remove the three overrides (`IsSymmetric`, `Key`, and `Iv`) and change the implementation so that your driver implements `BaseEncodingAsymmetricDriver` and `IEncodingAsymmetricDriver`.
{% endhint %}

#### Removed "HDD Uncleaner"

{% code title="KnownAddons.cs" lineNumbers="true" %}
```csharp
/// <summary>
/// HDD Uncleaner 2015 Legacy addon
/// </summary>
LegacyHddUncleaner,
```
{% endcode %}

We've removed an addon that provided you with an amusement app that simulated how an ancient software of ours, "HDD Cleaner", would run. It was only meant for demonstration purposes.

{% hint style="danger" %}
It's advisable to stop using this enumeration value, but the auto generator may override this enumerator's value with another addon.
{% endhint %}

#### Removed config property wrappers

As seen in [this commit](https://github.com/Aptivi/NitrocidKS/commit/e96c4559cf3b20079fc1f960579738fa81e522fd), we've decided to remove the configuration property wrappers as a result of the improvements that were witnessed in the configuration system during the development of the 0.1.0 version.

{% hint style="danger" %}
It's advised to use the `Config.MainConfig` instance to point to the required configuration properties that have their wrappers deleted.
{% endhint %}

#### Removed time/date corner

{% code title="KernelMainConfig.cs" lineNumbers="true" %}
```csharp
public bool CornerTimeDate { get; set; }
```
{% endcode %}

{% code title="TimeDateTopRight.cs" lineNumbers="true" %}
```csharp
public static class TimeDateTopRight
```
{% endcode %}

The time/date corner that was initially introduced in the 0.0.4.5 version of Nitrocid KS was considered to be one of the oldest features that stayed. Unfortunately, we had to remove this feature in favor of The Nitrocid Homepage.

{% hint style="danger" %}
It's advisable for you to stop using this feature.
{% endhint %}
