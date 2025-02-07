---
description: Follow the compatibility notes when upgrading your mods to API v1.2 series
---

# 🔼 Upgrading to API v1.2 series

When upgrading your modification from the target of the later version of Nitrocid KS that declares itself to be from the API v1.2, you must make necessary changes to be able to use your mod in a Nitrocid KS version which you use to test your mod. These changes will be listed starting from 0.0.12 to the last version in this API revision.

## From 0.0.11 to 0.0.12

This version added some of the interesting features, including the built-in kernel configuration tool that still witnessed improvements to this day.

{% content-ref url="../version-release-notes/v0.0.12.x-series.md" %}
[v0.0.12.x-series.md](../version-release-notes/v0.0.12.x-series.md)
{% endcontent-ref %}

### **`ProbeHW()`**

{% code title="HardwareProbe.vb" lineNumbers="true" %}
```visual-basic
Public Sub ProbeHW()
```
{% endcode %}

This function used to be a wrapper for hardware probing function which checked to see if the quiet mode was enabled on the kernel. However, it got removed.

{% hint style="danger" %}
We advice you to cease using this function, since hardware already gets probed at boot time.
{% endhint %}

### **`PreFetchNetworks` and `PostFetchNetworks` Events**

{% code title="EventsAndExceptions.vb" lineNumbers="true" %}
```visual-basic
'Events
Public Event PreFetchNetworks()
Public Event PostFetchNetworks()

'Responders
Public Sub RespondPreFetchNetworks() Handles Me.PreFetchNetworks
Public Sub RespondPostFetchNetworks() Handles Me.PostFetchNetworks

'Raisers
Public Sub RaisePreFetchNetworks()
Public Sub RaisePostFetchNetworks()
```
{% endcode %}

Each time the fetching network request is made, the above events get raised to notify the mods that fetching networks has commenced and ended. Since it was linked to the SAMBA-powered network fetcher that `lsnet` and `lsnettree` commands from 0.0.2 use, we decided to remove after being unused.

{% hint style="danger" %}
The new `lsnet` command currently doesn't provide these events, so we advice you to cease using these events until they're implemented again.
{% endhint %}

### **String Extensions**

{% code title="StringExtensions.vb" lineNumbers="true" %}
```visual-basic
Public Function ReplaceLastOccurrence(ByVal source As String, ByVal searchText As String, ByVal replace As String) As String
Public Iterator Function AllIndexesOf(ByVal str As String, ByVal value As String) As IEnumerable(Of Integer)
Public Function Truncate(ByVal str As String, ByVal threshold As Integer) As String
```
{% endcode %}

The three above string extensions were used across different parts of the kernel. We'll briefly summarize these functions one by one.

* `ReplaceLastOccurrence()` tries to replace only the last occurrence of the substring inside the target string with the text to be replaced.
* `AllIndexesOf()` gets all the indexes of the specified value from the string
* `Truncate()` tries to truncate the string after the specified threshold.

They were removed because Extensification was being made at that time, and they were included there. Furthermore, we've deprecated this library.

{% hint style="info" %}
To continue using these functions, we suggest that you re-implement these functions yourself.
{% endhint %}

## From 0.0.12 to 0.0.12.3

This version brought minor improvements. However, there was an API removal.

### `ListenRPC()`

{% code title="RemoteProcedure.vb" lineNumbers="true" %}
```visual-basic
Sub ListenRPC()
```
{% endcode %}

This function was called by the RPC thread which started during the call to the `StartRPC()` function. `ListenRPC()` tries to set the RPC listener up and start another thread responsible for receiving all available RPC commands from all connected devices. However, it got removed as it was merged to `StartRPC()`.

{% hint style="info" %}
StartRPC() and StopRPC() are still available as viable ways of starting and stopping the remote procedure server. The below code block shows you the method signature of the two functions.

{% code title="RemoteProcedure.cs" lineNumbers="true" %}
```csharp
public static void StartRPC()
public static void StopRPC()
```
{% endcode %}
{% endhint %}

## From 0.0.12.3 to 0.0.14

This version added some more features, like the usage of Inxi.NET for the first time in the kernel and the BouncingBlock screensaver.

{% content-ref url="../version-release-notes/v0.0.14.x-series.md" %}
[v0.0.14.x-series.md](../version-release-notes/v0.0.14.x-series.md)
{% endcontent-ref %}

### Hardware Classes and Routines

{% code title="HardwareVars.vb" lineNumbers="true" %}
```visual-basic
'Hardware classes and their variables
Public Class HDD
Public Class HDD_Linux
Public Class Part
Public Class Part_Linux
Public Class Logical
Public Class CPU
Public Class CPU_Linux
Public Class RAM
Public Class RAM_Linux
```
{% endcode %}

These above classes were used to deserialize all the hardware variables to their variables so their information can be accessed directly by mods.

{% code title="HardwareProbe.vb" lineNumbers="true" %}
```visual-basic
'Probers
Public Sub ProbeHardware()
Public Sub ProbeHardwareLinux()

'Listing
Sub ListDrivers_Linux()
Sub PrintDrives()
Sub PrintPartitions(ByVal Drive As Integer)
```
{% endcode %}

These are the probers and the listers used by the kernel to facilitate getting information about the hardware from your computer.

{% code title="" lineNumbers="true" %}
```visual-basic
'CPU feature classes
Public Class CPUFeatures_Win
Public Module CPUFeatures_Linux

'CPU feature query
Public Function CheckSSE(ByVal SSEVer As Integer) As Boolean
```
{% endcode %}

Finally, the above routines were used to check your processor for support of the Streaming SIMD Extensions (SSE) feature set.

As Inxi.NET got released at the end of 2020, it was used by Nitrocid KS to separate these above routines to the library so the hardware querying functions were no longer dependent on Nitrocid KS.

As a result, we removed all the above classes and routines.

{% hint style="info" %}
To continue using these functions and routines, use the Inxi.NET library. If you just want to list the hardware, use the `ListHardware()` routine.

{% code title="HardwareList.cs" lineNumbers="true" %}
```csharp
public static void ListHardware(string HardwareType)
```
{% endcode %}
{% endhint %}

### `DoCalc()`

{% code title="Calc.vb" lineNumbers="true" %}
```visual-basic
Public Function DoCalc(ByVal Expression As String) As Dictionary(Of Double, Boolean)
```
{% endcode %}

The calculator was re-implemented with the usage of the DataTable class to aid in solving the expression. This time, it only takes the expression and calculates it using the `Compute()` method from the abovementioned class. The expression had to follow the format of the `DataTable` expression that were described in the below link.

{% embed url="https://learn.microsoft.com/en-us/dotnet/api/system.data.datatable.compute?view=net-6.0" %}

It got removed again in favor of the string evaluator that was implemented in the `StringEvaluators` module, which was removed as of 0.1.0.

{% hint style="info" %}
The calculator no longer uses the string evaluator, and now uses the `StringMath` library to calculate the expressions.
{% endhint %}

## From 0.0.14 to 0.0.15

This version was released as the last major version in the API v1.2 series.

{% content-ref url="../version-release-notes/v0.0.15.x-series.md" %}
[v0.0.15.x-series.md](../version-release-notes/v0.0.15.x-series.md)
{% endcontent-ref %}

### `PrintLog()`

{% code title="DebugLogPrint.vb" lineNumbers="true" %}
```visual-basic
Sub PrintLog()
```
{% endcode %}

This function was used as a wrapper to the `ReadContents()` routine found in the Filesystem module. It got removed for being nothing but the wrapper for said method.

{% hint style="info" %}
To simulate this function, use the `ReadContents()` function and the console writer to print the debug logs. Both of the methods have their own method signatures written below in the code block.

{% code title="DebugLog.cs" lineNumbers="true" %}
```csharp
// Use DebuggingPaths from Paths.cs
public static string DebuggingPath

// and pass it to the filename argument of the below method from
// FileRead.cs
public static string[] ReadContents(string filename)

// Then, use the below method to write the lines to the console.
// Easiest way is to use the list writer. Full method list is found
// from ListWriterColor.cs
public static void WriteList<T>(IEnumerable<T> List)
```
{% endcode %}
{% endhint %}
