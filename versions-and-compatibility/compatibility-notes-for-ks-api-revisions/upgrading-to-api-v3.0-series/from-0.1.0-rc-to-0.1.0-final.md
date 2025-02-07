---
description: Guide for upgrading 0.1.0 RC mods to 0.1.0 Final
---

# ⬆️ From 0.1.0 RC to 0.1.0 Final

This page lists all the changes that have been made from 0.1.0 RC to 0.1.0 Final. For upgrading your mods from 0.0.24.x directly to the 0.1.0 series, use the main upgrade page where it highlights the most important changes.

## From RC to 0.1.0 Final

During the final version development period, we have made the following breaking changes:

### Enumerable tools moved to EnumMagic

{% code title="EnumerableTools.cs" lineNumbers="true" %}
```csharp
public static class EnumerableTools
```
{% endcode %}

We've moved `EnumerableTools`, which holds non-generic enumerable interface tools that make jobs easier, from Nitrocid to EnumMagic as we needed to make it more open to the public. This is to ensure that these extensions get used by different libraries, such as Terminaux.

{% hint style="info" %}
You'll need to reference EnumMagic in order to use the tools.
{% endhint %}

### Moved three hashing algorithms

<pre class="language-csharp" data-title="DriverHandler.cs" data-line-numbers><code class="lang-csharp">internal static Dictionary&#x3C;DriverTypes, Dictionary&#x3C;string, IDriver>> drivers = new()
{
    (...)
    {
        DriverTypes.Encryption, new()
        {
            { "Default", new SHA256() },
<strong>            { "MD5", new MD5() },
</strong><strong>            { "SHA1", new SHA1() },
</strong>            { "SHA256", new SHA256() },
<strong>            { "SHA384", new SHA384() },
</strong>            { "SHA512", new SHA512() }
        }
    },
    (...)
}
</code></pre>

The three hashing algorithms have been moved to their own individual addons:

* `Nitrocid.Extras.Md5`
* `Nitrocid.Extras.Sha1`
* `Nitrocid.Extras.Sha384`

As a result, your mods will break if they assume that these three algorithms are installed as built-in, which is the case for RC and earlier beta versions of 0.1.0. The reason of that is because of security.

{% hint style="info" %}
Your mods will break only if they have been used and the three addons are not installed yet.
{% endhint %}

### Removed `SettingVariable` command flag

{% code title="CommandFlags.cs" lineNumbers="true" %}
```csharp
public enum CommandFlags
{
    (...)
    [Obsolete("-set=varname already exists. Use the AcceptsSet parameter from the CommandArgumentInfo constructor instead of this flag.")]
    SettingVariable = 8,
    (...)
}
```
{% endcode %}

We have introduced a new way to give your UESH commands an option to set a value to the UESH variable that is set by the `-set=varname` switch. This way, we've decided to remove `SettingVariable` in favor of the `AcceptsSet` parameter from the `CommandArgumentInfo` constructor.

{% hint style="danger" %}
From now on, you'll no longer be able to use the `SettingVariable` switch.
{% endhint %}

### Figlet tools class moved to Terminaux 3.0

{% code title="FigletTextTools.cs" lineNumbers="true" %}
```csharp
public static class FigletTextTools
```
{% endcode %}

The figlet text tools class has been moved to Terminaux 3.0, because we were working on a major version of Terminaux to aim for better experience for console applications, which promises many features to come, such as the transparent console support.

As for this change, it has been moved to this version of the terminal library to be able to manage default Figlet fonts in easier and more consistent ways.

{% hint style="info" %}
From now, use the `Config.MainConfig.DefaultFigletFontName` property while Terminaux 3.0 gets released.
{% endhint %}

### Removed obsolete functions from 0.1.0 RC

{% code title="KernelThread.cs" lineNumbers="true" %}
```csharp
public ulong NativeThreadId
```
{% endcode %}

{% code title="AliasManager.cs" lineNumbers="true" %}
```csharp
public static void ManageAlias(string mode, ShellType Type, string AliasCmd, string DestCmd = "")
public static void ManageAlias(string mode, string Type, string AliasCmd, string DestCmd = "")
```
{% endcode %}

{% code title="UESHCommands.cs" lineNumbers="true" %}
```csharp
public static class UESHCommands
```
{% endcode %}

The above functions have been obsoleted during the development cycle of 0.1.0. They have been removed in preparation for the final release of 0.1.0 for the below reasons:

* The native thread ID can't be get accurately, because managed threads and native threads don't have a 1:1 relationship with each other. Furthermore, one managed thread may use multiple operating system native threads. It's obtainable through the diagnostic library created by Microsoft, ClrMd, but we don't want to risk implementing it in fear of malicious mods using them to cause problems, such as re-implementing `Thread.Abort()` of some sort.
* The `ManageAlias()` functions were wrappers of the alias management functions. The wrapper functions appear to be doing nothing other than just being proxy functions with the mode as string declaring if we need to add or to remove an alias.
* The `UESHCommands` class was created to help mod developers set a UESH variable if they had `SettingVariable` enabled. However, `-set=varname` was implemented, resulting in both `UESHCommands` and `SettingVariable` being candidates for removal.

As a result, we've decided to remove them to clean things up.

{% hint style="info" %}
Here are some tips:

* As for the native thread ID, it won't be re-implemented.
* As for the `ManageAlias()` functions, they won't be re-implemented.
* As for the UESHCommands class, you can just use the `-set=varname` switch instead.
{% endhint %}

### Exact wording definition changed

{% code title="CommandArgumentPart.cs" lineNumbers="true" %}
```csharp
public string ExactWording { get; private set; }
public CommandArgumentPart(bool argumentRequired, string argumentExpression, Func<string[], string[]> autoCompleter = null, bool isNumeric = false, string exactWording = null)
```
{% endcode %}

{% code title="CommandArgumentPartOptions.cs" lineNumbers="true" %}
```csharp
public string ExactWording { get; set; }
```
{% endcode %}

Earlier, we've introduced exact wording option to the command argument to let the user know that this is the exact wording that they must write in order for the command to get executed. However, doing the same thing for more than one word was tedious, as you had to make many instances of `CommandArgumentInfo` to cover all the words.

As a result, we've decided to turn the definition of the two above variables to an array of strings that indicates what one of the words the user used to execute a command.

{% hint style="info" %}
You'll have to turn the declaration of this variable from a simple string to an array of strings, even if it's only one word.

```diff
 [
     new CommandArgumentPart(true, "tui", new CommandArgumentPartOptions()
     {
-        ExactWording = "tui"
+        ExactWording = ["tui"]
...
```
{% endhint %}

### Moved slow/wrapped writers to `TextDynamicWriters`

{% code title="TextWriters.cs" lineNumbers="true" %}
```csharp
public static void WriteSlowly(string msg, bool Line, double MsEachLetter, KernelColorType colorType, params object[] vars)
public static void WriteSlowly(string msg, bool Line, double MsEachLetter, KernelColorType colorTypeForeground, KernelColorType colorTypeBackground, params object[] vars)
public static void WriteWhereSlowly(string msg, bool Line, int Left, int Top, double MsEachLetter, KernelColorType colorType, params object[] vars)
public static void WriteWhereSlowly(string msg, bool Line, int Left, int Top, double MsEachLetter, bool Return, KernelColorType colorType, params object[] vars)
public static void WriteWhereSlowly(string msg, bool Line, int Left, int Top, double MsEachLetter, bool Return, int RightMargin, KernelColorType colorType, params object[] vars)
public static void WriteWhereSlowly(string msg, bool Line, int Left, int Top, double MsEachLetter, KernelColorType colorTypeForeground, KernelColorType colorTypeBackground, params object[] vars)
public static void WriteWhereSlowly(string msg, bool Line, int Left, int Top, double MsEachLetter, bool Return, KernelColorType colorTypeForeground, KernelColorType colorTypeBackground, params object[] vars)
public static void WriteWhereSlowly(string msg, bool Line, int Left, int Top, double MsEachLetter, bool Return, int RightMargin, KernelColorType colorTypeForeground, KernelColorType colorTypeBackground, params object[] vars)
public static void WriteWrapped(string Text, bool Line, KernelColorType colorType, params object[] vars)
public static void WriteWrapped(string Text, bool Line, KernelColorType colorTypeForeground, KernelColorType colorTypeBackground, params object[] vars)
```
{% endcode %}

We have moved all the above writers from `TextWriters` to its own separate class that contains all the dynamic writers, `TextDynamicWriters`. The dynamic writers can not be stored as a single string, so we've moved them as appropriate.

{% hint style="info" %}
None of the functions have been changed. You just need to update the reference to `TextWriters` to point to `TextDynamicWriters` instead.
{% endhint %}

### Unsafe start/kill shell functions internalized

{% code title="ShellManager.cs" lineNumbers="true" %}
```csharp
public static void StartShellForced(ShellType ShellType, params object[] ShellArgs)
public static void StartShellForced(string ShellType, params object[] ShellArgs)
public static void KillShellForced()
```
{% endcode %}

Earlier beta versions of Nitrocid KS 0.1.0 have provided these functions to the public API space to allow your mods to forcefully start and/or kill shells, including the mother shell. However, we have realized that malicious mods can make use of these functions to try to log users out.

We took this into consideration and removed these functions from the public API space and reserved them for internal use.

{% hint style="warning" %}
As of the final release of Nitrocid KS 0.1.0, you'll no longer be able to kill the shells forcefully, including the mother shell.
{% endhint %}

### `LoginScreen()` now returns a boolean value

{% code title="ILoginHandler.cs" lineNumbers="true" %}
```csharp
void LoginScreen();
```
{% endcode %}

{% code title="BaseLoginHandler.cs" lineNumbers="true" %}
```csharp
public virtual void LoginScreen()
```
{% endcode %}

The login handler used to check to see if any login handler has requested a system shutdown, but the mod developers needed to make a global variable that checks to see if the user has requested a shutdown or reboot. Starting from the final version of Nitrocid 0.1.0, `LoginScreen()` now returns a boolean value to determine whether the login handler should proceed to go to the username selection screen (where you're required to input your username) or to refresh itself.

{% hint style="info" %}
If you hold this global variable, you must remove it and use the `return` clause to decide how to proceed.
{% endhint %}

### Overhauled the screensaver timeout settings

{% code title="KernelMainConfig.cs" lineNumbers="true" %}
```csharp
public int ScreenTimeout
```
{% endcode %}

{% code title="ScreensaverManager.cs" lineNumbers="true" %}
```csharp
public static int ScreenTimeout
```
{% endcode %}

The screensaver timeout settings has been changed to the TimeSpan instance to accommodate the large timeouts that are bigger than the integer maximum value (indicated by `int.MaxValue`). This also makes things easier than before by having you to specify the time span as in the `HH:MM:SS` format in its simplest form, or `DDD.HH:MM:SS.NNN` for completeness.

{% hint style="info" %}
Don't worry, the new implementation is smart enough to detect old configurations and to convert them (as in milliseconds) to the new format.
{% endhint %}

### Progress reporting simplified

{% code title="SplashReport.cs" lineNumbers="true" %}
```csharp
public static void ReportProgress(string Text, int Progress, bool force = false, ISplash splash = null, params object[] Vars)
```
{% endcode %}

We've made a refactor to the progress reporting codebase so that it's easier to maintain. As a consequence, two arguments from the warning and the error reporters have been added to the `ReportProgress()` function.

It may look intimidating to use, but it's rather easy, because all arguments after the `progress` number are optional. Also, we've implemented an enumeration list, called `SplashReportSeverity`, that allows you to report either a progress, a warning, or an error.

{% hint style="info" %}
You'll have to update your calls to the master `ReportProgress()` function to provide these two extra arguments before specifying the `ISplash` implementation:

* Exception: An exception to use (default is `null`)
* Severity: The severity of the report (default is `Info`)
{% endhint %}

### Removed opting in to new color selector option

{% code title="ConsoleTools.cs" lineNumbers="true" %}
```csharp
public static bool UseNewColorSelector
```
{% endcode %}

{% code title="KernelMainConfig.cs" lineNumbers="true" %}
```csharp
public bool UseNewColorSelector { get; set; } = true;
```
{% endcode %}

We've removed opting in to the new color selector because the old one was gone during the development of 0.1.0. We've also made the new color selector powered by Terminaux as default to be stronger and more accurate and visual than before.

This will make it easier to use the brand new color selector than the old one that started as a simple text.

{% hint style="danger" %}
You can no longer use this config entry. We suggest you use Terminaux's color selector feature.
{% endhint %}
