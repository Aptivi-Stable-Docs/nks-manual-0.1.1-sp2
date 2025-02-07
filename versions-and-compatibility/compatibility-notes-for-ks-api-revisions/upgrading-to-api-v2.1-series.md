---
description: Follow the compatibility notes when upgrading your mods to API v2.1 series
---

# 🔼 Upgrading to API v2.1 series

When upgrading your modification from the target of the later version of Nitrocid KS that declares itself to be from the API v2.1, you must make necessary changes to be able to use your mod in a Nitrocid KS version which you use to test your mod.

The following changes were listed sequentially as development went on.

## From 0.0.23 to 0.0.24

This version was released to make groundbreaking additions and improvements.

{% content-ref url="../version-release-notes/v0.0.24.x-series.md" %}
[v0.0.24.x-series.md](../version-release-notes/v0.0.24.x-series.md)
{% endcontent-ref %}

### **Removed support for `ICustomSaver`**

{% code title="ICustomSaver.vb" lineNumbers="true" %}
```visual-basic
<Obsolete("This custom screensaver interface is obsolete. Use IScreensaver and BaseScreensaver instead.")>
Public Interface ICustomSaver
```
{% endcode %}

{% code title="CustomSaverCompiler.vb" lineNumbers="true" %}
```visual-basic
Public Sub CompileCustom(file As String)
```
{% endcode %}

Custom screensavers used to implement the `ICustomSaver` interface to implement the screensaver logic. Now, it has been removed in favor of the new screensaver class implementing the newer `IScreensaver` interface, `BaseScreensaver`.

This breaks all the existing screensavers that still use the old `ICustomSaver` interface. This has also resulted in the removal of the `Custom()` function and reimplementation as `ParseCustomSaver`.

{% hint style="info" %}
All new screensavers should use the `BaseScreensaver` class. All existing screensavers should migrate from `ICustomSaver` to `BaseScreensaver`.

As for the parser, it has been reimplemented to take only the screensaver DLL files.
{% endhint %}

### **Removed support for `PerformCmd()`**

{% code title="IScript.vb" lineNumbers="true" %}
```visual-basic
Sub PerformCmd(Command As CommandInfo, Optional Args As String = "")
```
{% endcode %}

`CommandBase.Execute()` was implemented to replace the above function, so we decided to remove the function as it only supported one command at a time, and you had to make a switch case statement for each command executed.

{% hint style="info" %}
`BaseCommand.Execute()` can be overridden in the below method signature:

{% code title="BaseCommand.cs" lineNumbers="true" %}
```csharp
public virtual void Execute(string StringArgs, string[] ListArgsOnly, string[] ListSwitchesOnly)
```
{% endcode %}
{% endhint %}

### **Restructured the filesystem API**

Filesystem module was a _**god class**_, so we decided to consolidate it to their own separate files to accomodate with the upcoming changes. However, you need to use their own namespace (for example, if you want to copy a file, import `KS.Files.Operations`) to be able to use them.

{% code title="Filesystem.vb" lineNumbers="true" %}
```visual-basic
Public Function SetSizeParseMode(Enable As Boolean) As Boolean
```
{% endcode %}

We have also removed `SetSizeParseMode()` as it's redundant and it was there for compatibility reasons.

{% hint style="info" %}
The base `Filesystem` module will stay so that path neutralization and invalid path detection routines will still be available under the same namespace in 0.0.24.0 and above.
{% endhint %}

* Removed functions:
  * `SetSizeParseMode()`
* New namespaces:
  * `KS.Files.Attributes`
  * `KS.Files.Folders`
  * `KS.Files.LineEndings`
  * `KS.Files.Operations`
  * `KS.Files.PathLookup`
  * `KS.Files.Print`
  * `KS.Files.Querying`
  * `KS.Files.Read`

{% hint style="info" %}
Choose one of the above namespaces to select the kind of filesystem operation you're going to do. However, these namespaces were written at the time of the change commit date, so the API documentation always gets up-to-date.
{% endhint %}

### **Separated `ConsoleColors` enumeration**

{% code title="Color255.vb" lineNumbers="true" %}
```visual-basic
Public Enum ConsoleColors As Integer
```
{% endcode %}

This enumeration used to hold all the 255 console colors and their names for easier selection. It was just separated from the `Color255` module to put it directly to the `KS.ConsoleBase` namespace

{% hint style="info" %}
You can still use this enumeration in the Terminaux library.
{% endhint %}

### **Removed `KS.Misc.Dictionary` to substitute with Dictify**

{% code title="DictionaryManager.vb" lineNumbers="true" %}
```visual-basic
Public Function GetWordInfo(Word As String) As DictionaryWord
```
{% endcode %}

{% code title="DictionaryWord.vb" lineNumbers="true" %}
```visual-basic
Public Class DictionaryWord
```
{% endcode %}

As Dictify got released, we decided to remove these functions and use them from the library.

{% hint style="info" %}
You can still use these functions using the Dictify library.
{% endhint %}

### **Moved APIs to their own namespace**

We have moved the below APIs to the below namespaces as they keep getting expanded:

* Network transfer APIs -> `KS.Network.Transfer`
* Color console APIs -> `KS.ConsoleBase.Themes`
* Input console APIs -> `KS.ConsoleBase.Inputs`

### **Removed the progress and its report positions from `SplashInfo`**

{% code title="SplashInfo.vb" lineNumbers="true" %}
```visual-basic
Public ReadOnly Property ProgressWritePositionX As Integer
Public ReadOnly Property ProgressWritePositionY As Integer
Public ReadOnly Property ProgressReportWritePositionX As Integer
Public ReadOnly Property ProgressReportWritePositionY As Integer
```
{% endcode %}

These four progress position variables were used to indicate where the splash manager should write the progress percentage and progress report. Splashes are now responsible for these.

{% hint style="info" %}
We advice you to assign these variables in your splash code instead of using these fields.
{% endhint %}

### **Replaced `CommandPromptWrite()`**

{% code title="UESHShell.vb" lineNumbers="true" %}
```visual-basic
Public Sub CommandPromptWrite()
```
{% endcode %}

`CommandPromptWrite()` used to be the helper for UESH to write its own prompt, but it's eventually replaced by the more powerful `WriteShellPrompt()` to accommodate all possible cases, like custom variables, and so on.

{% hint style="info" %}
Your shell should use the `PromptPresetBase` class to define your own shell prompts.

{% code title="PromptPresetManager.cs" lineNumbers="true" %}
```csharp
public static void WriteShellPrompt(ShellType ShellType)
public static void WriteShellPrompt(string ShellType)
```
{% endcode %}
{% endhint %}

### **Organized the `ShellBase` namespaces**

The shell base has been divided to three types:

* Aliases (namespace: `KS.Shell.ShellBase.Aliases`)
* Commands (namespace: `KS.Shell.ShellBase.Commands`)
* Shells (namespace: `KS.Shell.ShellBase.Shells`)

{% hint style="info" %}
Use the above namespaces if you want to perform operations in your own custom shell.
{% endhint %}

### **Changed progress bar writer module name**

{% code title="ProgressColor.vb" lineNumbers="true" %}
```visual-basic
Public Module ProgressColor
```
{% endcode %}

We didn't want to cause confusion between `ProgressColor` in the `FancyWriters` namespace and the actual `ProgressColor` in the `ColorTools` module, so we decided to extend the name of the progress bar writer module to `ProgressBarColor`.

{% hint style="info" %}
To utilize the progress bar writers, you need to use the `KS.Misc.Writers.FancyWriters` namespace and use the `ProgressBarColor` class methods.
{% endhint %}
