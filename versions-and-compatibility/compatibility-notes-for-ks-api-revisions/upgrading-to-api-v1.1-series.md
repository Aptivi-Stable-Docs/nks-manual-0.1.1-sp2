---
description: Follow the compatibility notes when upgrading your mods to API v1.1 series
---

# 🔼 Upgrading to API v1.1 series

When upgrading your modification from the target of the later version of Nitrocid KS that declares itself to be from the API v1.1, you must make necessary changes to be able to use your mod in a Nitrocid KS version which you use to test your mod. These changes will be listed starting from 0.0.8 to the last version in this API revision.

## From 0.0.7 to 0.0.8

This version brought many awesome changes, including the addition of screensavers and SSH.

{% content-ref url="../version-release-notes/v0.0.8.x-series.md" %}
[v0.0.8.x-series.md](../version-release-notes/v0.0.8.x-series.md)
{% endcontent-ref %}

### **`CheckSSEs()`**

{% code title="CPUFeatures.vb" lineNumbers="true" %}
```visual-basic
Public Sub CheckSSEs()
```
{% endcode %}

This function was used to get information about your processor's support of the Streaming SIMD Extension (SSE) versions. Since all of today's computers come loaded with SSE2 support, we felt that it was an unnecessary addition, so we removed it. However, it came back in Inxi.NET.

{% hint style="info" %}
You can use Inxi.NET's CPU Flags property to get advantage of this feature.
{% endhint %}

### Directory Structure Initialization

{% code title="Filesystem.vb" lineNumbers="true" %}
```visual-basic
Public Sub InitStructure()
Private Sub UACNoticeShow()
```
{% endcode %}

According to the debug logs inside the function, it was meant to make a list of all the files and directories in the current directory. However, it runs each startup, causing the boot times of the kernel to increase. Also, the populated filesystem list didn't get used across the entire filesystem routine. This caused us to remove the entire functionality.

{% hint style="danger" %}
We advice you to cease using this function, since it's classed as abusive.
{% endhint %}

## From 0.0.8 to 0.0.10

This release brings a vast amount of improvements, but there is one API removal which is pinpointed below.

{% content-ref url="../version-release-notes/v0.0.10.x-series.md" %}
[v0.0.10.x-series.md](../version-release-notes/v0.0.10.x-series.md)
{% endcontent-ref %}

### `DisconnectDbgDevCmd()`

{% code title="RemoteDebugger.vb" lineNumbers="true" %}
```visual-basic
Sub DisconnectDbgDevCmd(ByVal IPAddr As String)
```
{% endcode %}

This function was released to make a wrapper for the `DisconnectDbgDev()` function. However, as it was reserved for running in commands according to `GetCommand.vb`, it was removed for being a duplicate.

{% hint style="info" %}
Alternatively, you can use the same function in the latest kernel version. `DisconnectDbgDev()` in API 3.0 can handle these cases. The method signature is shown in the below code block.

{% code title="RemoteDebugTools.cs" lineNumbers="true" %}
```csharp
public static void DisconnectDbgDev(string IPAddr)
```
{% endcode %}
{% endhint %}

## From 0.0.10 to 0.0.11

This kernel version series added new features, like the text editor.

{% content-ref url="../version-release-notes/v0.0.11.x-series.md" %}
[v0.0.11.x-series.md](../version-release-notes/v0.0.11.x-series.md)
{% endcontent-ref %}

### `InitFS()`

{% code title="Filesystem.vb" lineNumbers="true" %}
```visual-basic
Public Sub InitFS()
```
{% endcode %}

This function used to call the filesystem structure population which was removed earlier for abuse back in 0.0.4.5. We mean by abuse in this context the hard drive abuse in which the kernel attempts to list all directories and files under the current directory each time the kernel starts. It was removed later for being nothing more than a shortcut for setting the current directory to the home directory.

{% hint style="danger" %}
We advice you to cease using this function.
{% endhint %}

### `FTPNotEnoughArgumentsException` class

{% code title="EventsAndExceptions.vb" lineNumbers="true" %}
```visual-basic
Public Class FTPNotEnoughArgumentsException
```
{% endcode %}

This exception used to be thrown when the user didn't provide enough arguments for any FTP command that needed it. Since it went unused, it was removed multiple times; once in this version, and the last time as part of the kernel exception improvement action.

{% hint style="danger" %}
Since this exception got removed twice, we advice you to cease using this class.
{% endhint %}

### `JsonNullException` class

{% code title="EventsAndExceptions.vb" lineNumbers="true" %}
```visual-basic
Public Class JsonNullException
```
{% endcode %}

This function was thrown each time the currency converter, which was removed in the  0.0.6.x series, reported an invalid JSON string. It was removed as a result of it being unused.

{% hint style="danger" %}
We advice you to cease using this class.
{% endhint %}

### `TruncatedManpageException` class

{% code title="EventsAndExceptions.vb" lineNumbers="true" %}
```visual-basic
Public Class TruncatedManpageException
```
{% endcode %}

This exception used to be thrown when the manual page handler detected that the manual page was truncated or had an invalid format. It was now removed because it went unused, too.

{% hint style="info" %}
`InvalidManpage` returned after the manual page parser and viewer returned.
{% endhint %}

### `Speak()`

{% code title="VoiceManagement.vb" lineNumbers="true" %}
```visual-basic
Public Sub Speak(ByVal Text As String)
```
{% endcode %}

Speech function was added in the first version of KS API v1.1, 0.0.8, to aid in trying to make a speech synthesizer for the kernel. We kept running into instabilities because we tried to maintain the cross-platform method of converting text to voice using the Google Translation API after we failed to get the built-in Windows-only voice handler, which was shipped with .NET Framework, to work in Linux.

It was also tried with NAudio in earlier milestones of the version to no avail because that library wasn't designed to work with Linux. The reason behind that is that it uses wrappers for available sound libraries which only exist in Windows, like WaveOut, DirectX's DirectSound, and so on.

Therefore, we decided to remove the entire synthesizer from the kernel as the simulator was no longer biased to Windows systems.

{% hint style="danger" %}
Unless there is a specialized sound library that is cross-platform, we advice you to cease using this function.
{% endhint %}
