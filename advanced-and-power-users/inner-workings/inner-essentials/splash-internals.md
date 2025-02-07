---
description: Splash screens!
icon: droplet
---

# Splash Internals

<figure><img src="../../../.gitbook/assets/001-welcome.png" alt=""><figcaption></figcaption></figure>

Splash screens are usually shown when the kernel reaches to the point that the pre-boot environment is no longer needed. Think of these splash screens as Plymouth splash screens that appear on Debian, or the Windows logo that appears when Windows starts. Nitrocid attempts to simulate the same concept.

The splash tools consists of the following classes:

* `SplashManager`: A class that handles splashes
* `SplashReport`: A class that handles reporting splashes

Splashes also contain a context to tell the splash screen that it's in a specific mode, which is one of the following:

* `Showcase`: This is used to showcase your splash screen and how well it works. Used by the `previewsplash` for best experience.
* `StartingUp`: Used by the kernel to indicate that the kernel is starting up.
* `ShuttingDown`: Used by the kernel to indicate that the kernel is shutting down.
* `Preboot`: Used by the kernel to indicate that the kernel is currently on the pre-boot stage before configuration is loaded.

## Splash Management

The splash management class contains several useful tools to manage them, like loading custom splashes, unloading them, previewing them, etc. The three commonly used functions are:

### Registering the splashes

{% code title="SplashManager.cs" lineNumbers="true" %}
```csharp
public static void RegisterSplash(SplashInfo splashInfo)
```
{% endcode %}

This function registers your splash from the `SplashInfo` instance that you've defined in your mod. This allows your mods that have the load priority set to `Important` to register the splashes before the configuration system loads your config.

{% hint style="info" %}
Since the main kernel configuration file holds the splash info, we suggest that you set your mod loading importance to `Important`.
{% endhint %}

### Removing the splashes

{% code title="SplashManager.cs" lineNumbers="true" %}
```csharp
public static void UnregisterSplash(string splashName)
```
{% endcode %}

This function removes your splash from the list of known splashes. This function takes the name of your custom splash, usually taken from your splash's `SplashInfo` instance.

### Checking your splash

{% code title="SplashManager.cs" lineNumbers="true" %}
```csharp
public static bool IsSplashRegistered(string splashName)
```
{% endcode %}

This function checks the list of splashes to query the splash name for its existence. If the splash exists, it returns true. Otherwise, false is returned.

## Showing Messages during Kernel Boot

There are ways to show the messages during kernel boot when splashes are enabled.

{% hint style="warning" %}
In your mod start code, please use one of the following `ReportProgress()` functions to let the splash know that your mod is making progress. Don't call any of the console printing functions, since they don't log and may mess up the splash screen, depending on the splash.
{% endhint %}

### Reporting progress

{% code title="SplashReport.cs" lineNumbers="true" %}
```csharp
public static void ReportProgress(string Text, params object[] Vars)
public static void ReportProgress(string Text, int Progress, params object[] Vars)
public static void ReportProgress(string Text, int Progress, bool force = false, Exception exception = null, SplashReportSeverity severity = SplashReportSeverity.Info, ISplash splash = null, params object[] Vars)
```
{% endcode %}

The above functions let you report progress to the splash displayer when the kernel is booting. These messages are passed to the normal progress writer found in the current splash instance, which decides how to display it.

The splash report severity in the last overload, SplashReportSeverity, has the following values:

* `Info`
* `Warning`
* `Error`

{% hint style="info" %}
The first function overload for reporting normal progress doesn't increment the progress; it only reports the message to the splash screen for it to display it or to ignore it, depending on the splash.
{% endhint %}

### Reporting warnings

{% code title="SplashReport.cs" lineNumbers="true" %}
```csharp
public static void ReportProgressWarning(string Text, params object[] Vars)
public static void ReportProgressWarning(string Text, Exception exception, params object[] Vars)
public static void ReportProgressWarning(string Text, bool force = false, ISplash splash = null, Exception exception = null, params object[] Vars)
```
{% endcode %}

The above functions let you report warnings to the splash displayer when the kernel is booting. These messages are passed to the warning progress writer found in the current splash instance, which decides how to display it.

### Reporting errors

{% code title="SplashReport.cs" lineNumbers="true" %}
```csharp
public static void ReportProgressError(string Text, params object[] Vars)
public static void ReportProgressError(string Text, Exception exception, params object[] Vars)
public static void ReportProgressError(string Text, bool force = false, ISplash splash = null, Exception exception = null, params object[] Vars)
```
{% endcode %}

The above functions let you report errors to the splash displayer when the kernel is booting. These messages are passed to the error progress writer found in the current splash instance, which decides how to display it.

### Resetting progress report

{% code title="SplashReport.cs" lineNumbers="true" %}
```csharp
public static void ResetProgressReportArea()
public static void ResetProgressReportArea(ISplash splash = null)
```
{% endcode %}

The above functions reset the progress report so that it says "Loading," so these functions don't actually clear out the progress report. If you have nothing else to report in the splash, you can use these functions.

### Important messages

{% code title="SplashManager.cs" lineNumbers="true" %}
```csharp
public static void BeginSplashOut()
public static void BeginSplashOut(SplashContext context)
public static void EndSplashOut()
public static void EndSplashOut(SplashContext context)
```
{% endcode %}

If you want to show messages or anything interesting during the kernel boot, you may want to use both the `BeginSplashOut()` and `EndSplashOut()` functions, surrounding both with code to show a message inside. Here's a simple example to show a test message:

{% code title="SplashManager.cs" lineNumbers="true" %}
```csharp
SplashManager.BeginSplashOut();
InfoBoxColor.WriteInfoBox(Translate.DoTranslation("We've reached {0}%!"), vars: prog);
SplashManager.EndSplashOut();
```
{% endcode %}

### Boot log buffers

The kernel stores a short boot log buffer for each session. You can get the boot log by going to the administrative shell and executing the `bootlog` command.

<figure><img src="../../../.gitbook/assets/132-inner.png" alt=""><figcaption></figcaption></figure>

You can also access the `LogBuffer` property in the `SplashReport` class, which is defined like this:

{% code title="SplashReport.cs" lineNumbers="true" %}
```csharp
public static string[] LogBuffer =>
    logBuffer.ToArray();
```
{% endcode %}

{% hint style="info" %}
Even if the splash doesn't display any progress messages, they are still saved to the boot log buffers for reference.
{% endhint %}
