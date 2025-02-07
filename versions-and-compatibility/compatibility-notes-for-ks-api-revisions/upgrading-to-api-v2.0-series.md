---
description: Follow the compatibility notes when upgrading your mods to API v2.0 series
---

# 🔼 Upgrading to API v2.0 series

When upgrading your modification from the target of the later version of Nitrocid KS that declares itself to be from the API v2.0, you must make necessary changes to be able to use your mod in a Nitrocid KS version which you use to test your mod. These changes will be listed starting from 0.0.20 to the last version in this API revision.

The following changes were listed sequentially as development went on.

## From 0.0.18 to 0.0.20

This version was released to make groundbreaking additions and improvements.

{% content-ref url="../version-release-notes/v0.0.20.x-series.md" %}
[v0.0.20.x-series.md](../version-release-notes/v0.0.20.x-series.md)
{% endcontent-ref %}

### **Unified help system to support every shell**

{% code lineNumbers="true" %}
```visual-basic
Public Sub TestShowHelp(Optional ByVal command As String = "")
Public Sub SFTPShowHelp(Optional ByVal command As String = "")
Public Sub RDebugShowHelp(ByVal command As String, ByVal DeviceStream As StreamWriter)
Public Sub RSSShowHelp(Optional ByVal command As String = "")
Public Sub IMAPShowHelp(Optional ByVal command As String = "")
Public Sub FTPShowHelp(Optional ByVal command As String = "")
Public Sub ZipShell_GetHelp(Optional ByVal Command As String = "")
Public Sub TextEdit_GetHelp(Optional ByVal Command As String = "")
```
{% endcode %}

The help system used to provide functions for each shell all showing a help entry for a provided command. They were separated for each shell because they queried commands.

They're now unified to one help function. As a result, all above functions were removed.

{% hint style="info" %}
You can use the `ShowHelp()` function to utilize the feature. The below method signatures show their usage in your mods.

{% code title="HelpSystem.cs" lineNumbers="true" %}
```csharp
public static void ShowHelp()
public static void ShowHelp(ShellType CommandType)
public static void ShowHelp(string command)
public static void ShowHelp(string command, ShellType CommandType)
public static void ShowHelp(string command, string CommandType)
```
{% endcode %}
{% endhint %}

### **Improved naming of injected commands**

{% code title="ArgumentParse.vb" lineNumbers="true" %}
```visual-basic
Public argcommands As String
Public argcmds() As String
```
{% endcode %}

The kernel argument parser used to host these two variables; the first one was the argument commands input, and the second one was the argument command array. However, the argument parser was re-written, so they were replaced with an array.

{% hint style="danger" %}
The kernel arguments were later removed, leaving only the command-line arguments. We suggest you to cease using this function.
{% endhint %}

### Prefixed the FTP shell variables with "Ftp"

{% code title="FTPShell.vb" lineNumbers="true" %}
```visual-basic
Public connected As Boolean = False
Private initialized As Boolean = False
Public currDirect As String 'Current Local Directory
Public currentremoteDir As String 'Current Remote Directory
Public user As String
Friend pass As String
Private strcmd As String
```
{% endcode %}

These variables were used by the FTP client, each having their own use.

* The `connected` variable used to be an indicator of when the FTP client was connected or not.
* The `initialized` variable used to be an indicator of when the FTP client initialized its logging function and other things.
* The `currDirect` variable used to tell the FTP client where is the local directory.
* The `currentRemoteDir` variable used to tell the FTP client where is the remote directory.
* The `user` variable hosted the connected username
* The private `password` variable hosted the password of the username
* The private `strcmd` variable was used to tell the FTP client what is the command

Each of these variables were given the `Ftp` prefix.

{% hint style="info" %}
The variables were moved to `FtpShellCommon` module.
{% endhint %}

### Relocated `Client(S)FTP` to their `Shell.vb` files

{% code lineNumbers="true" %}
```visual-basic
'FTP client on FTPGetCommand.vb
Public ClientFTP As FtpClient

'SFTP client on SFTPGetCommand.vb
Public ClientSFTP As SftpClient
```
{% endcode %}

These variables were used to host the FTP and SFTP clients to perform operations related to the client. However, they're moved to their shell code.

{% hint style="info" %}
These variables are still accessible, though they're now properties. To get the actual client from `NetworkConnection`, you need to cast the `ConnectionInstance` of the two below properties to their respective types: `FtpClient`, `SftpClient`.

{% code lineNumbers="true" %}
```csharp
// FTP client on FTPShellCommon.cs
public static NetworkConnection ClientFTP

// SFTP client on SFTPShellCommon.cs
public static NetworkConnection ClientSFTP
```
{% endcode %}
{% endhint %}

### **Reworked on how to create notifications**

{% code title="Notifications.vb" lineNumbers="true" %}
```visual-basic
Public Function NotifyCreate(ByVal Title As String, ByVal Desc As String, ByVal Priority As NotifPriority, ByVal Type As NotifType) As Notification
Public Class Notification
```
{% endcode %}

The notification system was created to manage notifications. Each notification have their own class outlined in the second line, created by the `NotifyCreate()` function. Because of a rework, the function was made obsolete and the Notification class was moved to its own file.

{% hint style="info" %}
The below notification constructor can be used to create a new notification.

{% code title="Notification.cs" lineNumbers="true" %}
```csharp
public Notification(string Title, string Desc, NotificationManager.NotifPriority Priority, NotificationManager.NotifType Type)
```
{% endcode %}
{% endhint %}

### Made getting kernel paths more secure

{% code title="KernelTools.vb" lineNumbers="true" %}
```visual-basic
Public paths As New Dictionary(Of String, String)
```
{% endcode %}

An array, `paths`, used to store all the kernel paths, but it was implemented insecurely, so we decided to replace it with the `GetKernelPath()` and `GetOtherPath()` functions.

{% hint style="info" %}
In the current version, only the `GetKernelPath()` function can be used.

{% code title="Paths.cs" lineNumbers="true" %}
```csharp
public static string GetKernelPath(KernelPathType PathType)
```
{% endcode %}
{% endhint %}

### **Debug now uses the `DebugLevel` enumerator**

{% code title="DebugWriters.vb" lineNumbers="true" %}
```visual-basic
Public Sub Wdbg(ByVal Level As Char, ByVal text As String, ByVal ParamArray vars() As Object)
Public Sub WdbgConditional(ByRef Condition As Boolean, ByVal Level As Char, ByVal text As String, ByVal ParamArray vars() As Object)
Public Sub WdbgDevicesOnly(ByVal Level As Char, ByVal text As String, ByVal ParamArray vars() As Object)
```
{% endcode %}

The debugging functions used to take a debugging level using characters, but it has been changed to the `DebugLevel` enumerator to limit the characters which can be used.

{% hint style="info" %}
The debugger still uses the debug error level using the enumeration mentioned above. Updated versions of the kernel can use these debug writers with the enumerator, by referring to the method signatures of these.

{% code title="DebugWriter.cs" lineNumbers="true" %}
```csharp
public static void WriteDebug(DebugLevel Level, string text, params object[] vars)
public static void WriteDebugConditional(bool Condition, DebugLevel Level, string text, params object[] vars)
public static void WriteDebugDevicesOnly(DebugLevel Level, string text, params object[] vars)
```
{% endcode %}
{% endhint %}

### Rewritten the command handler

{% code lineNumbers="true" %}
```visual-basic
'Found in FTPGetCommand.vb
Public Sub ExecuteCommand(ByVal requestedCommand As String)

'Found in SFTPGetCommand.vb
Public Sub ExecuteCommand(ByVal requestedCommand As String)
```
{% endcode %}

These two functions were implemented separately for FTP and SFTP shells, because at the time of the development, the main shell command executor wasn't ready to support shells other than the main UESH shell.

```visual-basic
Public SFTPStartCommandThread As New Thread(AddressOf ExecuteCommand) With {.Name = "SFTP Command Thread"}
Public RSSCommandThread As New Thread(AddressOf RSSParseCommand) With {.Name = "RSS Shell Command Thread"}
Public MailStartCommandThread As New Thread(AddressOf Mail_ExecuteCommand) With {.Name = "Mail Command Thread"}
Public FTPStartCommandThread As New Thread(AddressOf ExecuteCommand) With {.Name = "FTP Command Thread"}
Public TextEdit_CommandThread As New Thread(AddressOf TextEdit_ParseCommand) With {.Name = "Text Edit Command Thread"}
Public ZipShell_CommandThread As New Thread(AddressOf ZipShell_ParseCommand) With {.Name = "ZIP Shell Command Thread"}
Public TStartCommandThread As New Thread(AddressOf TParseCommand) With {.Name = "Test Shell Command Thread"}
Public StartCommandThread As New Thread(AddressOf ExecuteCommand) With {.Name = "Shell Command Thread"
```

These threads were used to handle shell execution.

The two command executors mentioned above were removed and the command threads were moved to ShellStartThreads as a result of recent improvements related to the command handler.

{% hint style="info" %}
The command executors can be invoked by using the `GetLine()` method for your own shells implemented in your mods.

{% code title="Shell.cs" lineNumbers="true" %}
```csharp
public static void GetLine()
public static void GetLine(string FullCommand)
public static void GetLine(string FullCommand, string OutputPath = "")
public static void GetLine(string FullCommand, string OutputPath = "", ShellType ShellType = ShellType.Shell)
public static void GetLine(string FullCommand, string OutputPath = "", string ShellType = "Shell", bool restoreDriver = true)
```
{% endcode %}
{% endhint %}

### Moved platform detection methods to PlatformDetector

{% code title="Kernel.vb" lineNumbers="true" %}
```visual-basic
Public Function IsOnWindows() As Boolean
Public Function IsOnUnix() As Boolean
Public Function IsOnMacOS() As Boolean
```
{% endcode %}

These functions were used to detect the platform configuration. For easier access, we've moved these methods to PlatformDetector.

{% hint style="info" %}
You can still access these functions, though they're now moved to `KernelPlatform`.
{% endhint %}

### **Split `ICustomSaver` to separate codefile**

{% code title="Screensaver.vb" lineNumbers="true" %}
```visual-basic
Public Interface ICustomSaver
```
{% endcode %}

This interface was made to support the custom screensavers. It has been split from the master class to the separate interface that grants easier access.

{% hint style="info" %}
You can still use this interface, though it's renamed to `IScreensaver`.

{% code title="IScreensaver.cs" lineNumbers="true" %}
```csharp
public interface IScreensaver
```
{% endcode %}
{% endhint %}

### **Renamed variables in public API**

{% code lineNumbers="true" %}
```visual-basic
'Screensaver.vb
Public defSaverName As String
Public CSvrdb As New Dictionary(Of String, ICustomSaver)
Public finalSaver As ICustomSaver
Public ReadOnly ScrnSvrdb As New Dictionary(Of String, BackgroundWorker)

'ColorTools.vb
Public colorTemplates As New Dictionary(Of String, ThemeInfo)

'Flags.vb
Public setRootPasswd As Boolean
Public RootPasswd As String
Public maintenance As Boolean
Public argsOnBoot As Boolean
Public clsOnLogin As Boolean
Public showMOTD As Boolean
Public simHelp As Boolean
Public slotProbe As Boolean
Public CornerTD As Boolean
Public instanceChecked As Boolean

'Kernel.vb
Public HName As String

'Translate.vb
Public currentLang As String

'DebugWriters.vb
Public dbgWriter As StreamWriter
Public dbgStackTraces As New List(Of String)

'FTPShell.vb
Public ftpsite As String
Public ftpexit As Boolean

'RemoteDebugger.vb
Public dbgConns As New Dictionary(Of StreamWriter, String)

'SFTPShell.vb
Public sftpsite As String
Public sftpexit As Boolean

'NetworkTools.vb
Public DRetries As Integer
Public URetries As Integer

'Shell.vb
Public modcmnds As New ArrayList
```
{% endcode %}

These variables were used for different purposes. They've been renamed to the following:

* `defSaverName` -> `DefSaverName`
* `CSvrdb` -> `CustomSavers`
* `finalSaver` -> `FinalSaver`
* `ScrnSvrdb` -> `Screensavers`
* `colorTemplates` -> `ColorTemplates`
* `setRootPasswd` -> `SetRootPassword`
* `RootPasswd` -> `RootPassword`
* `maintenance` -> `Maintenance`
* `argsOnBoot` -> `ArgsOnBoot`
* `clsOnLogin` -> `ClearOnLogin`
* `showMOTD` -> `ShowMOTD`
* `simHelp` -> `SimHelp`
* `slotProbe` -> `SlotProbe`
* `CornerTD` -> `CornerTimeDate`
* `instanceChecked` -> `InstanceChecked`
* `HName` -> `HostName`
* `currentLang` -> `CurrentLanguage`
* `dbgWriter` -> `DebugWriter`
* `dbgStackTraces` -> `DebugStackTraces`
* `ftpsite` -> `FtpSite`
* `ftpexit` -> `FtpExit`
* `dbgConns` -> `DebugConnections`
* `sftpsite` -> `SFTPSite`
* `sftpexit` -> `SFTPExit`
* `DRetries` -> `DownloadRetries`
* `URetries` -> `UploadRetries`
* `modcmnds` -> `ModCommands`

{% hint style="info" %}
Some of these flags were remade to properties in the latest kernel API, and some of them were removed.
{% endhint %}

### **Made some cleanups regarding MOTD parser**

{% code title="MOTDParse.vb" lineNumbers="true" %}
```visual-basic
Public MOTDStreamR As IO.StreamReader
Public MOTDStreamW As IO.StreamWriter
Public Sub ReadMOTDFromFile(ByVal MType As MessageType)
```
{% endcode %}

The first two stream variables were used by `ReadMOTDFromFile` and `SetMOTD` respectively. These variables were removed and the reader function was renamed to `ReadMOTD`.

{% hint style="info" %}
The `ReadMotd()` function was still available, but it has been separated to `ReadMal` and this function. Their method signatures are found below.

```csharp
// MotdParse.cs
public static void ReadMotd()

// MalParse.cs
public static void ReadMal()
```
{% endhint %}

### Split `GetConnectionInfo`

{% code title="SSH.vb" lineNumbers="true" %}
```visual-basic
Public Function GetConnectionInfo(ByVal Address As String, ByVal Port As Integer, ByVal Username As String) As ConnectionInfo
```
{% endcode %}

This function used to host both the connection info constructor and the prompt for the SSH client. It has been split to two functions: `GetConnectionInfo` and `PromptConnectionInfo` to add the `AuthMethods` argument in the former function.

{% hint style="info" %}
These functions can be used to construct SSH connection information.

{% code title="SSH.cs" lineNumbers="true" %}
```csharp
public static ConnectionInfo GetConnectionInfo(string Address, int Port, string Username, List<AuthenticationMethod> AuthMethods)
public static ConnectionInfo PromptConnectionInfo(string Address, int Port, string Username)
```
{% endcode %}
{% endhint %}

### **Changed how mail listing works**

{% code title="MailManager.vb" lineNumbers="true" %}
```visual-basic
Public Function MailListMessages(ByVal PageNum As Integer) As String
```
{% endcode %}

This function used to construct a string containing a list of messages according to the page number. It used `StringBuilder` to build such a string. However, this has proven to be unreliable, so we decided to change how it works by directly printing the list to the console.

{% hint style="info" %}
This function is still available. They can be used with these method signatures.

{% code title="MailManager.cs" lineNumbers="true" %}
```csharp
public static void MailListMessages(int PageNum)
public static void MailListMessages(int PageNum, int MessagesInPage)
```
{% endcode %}
{% endhint %}

### **Changed how reading contents API works**

{% code title="Filesystem.vb" lineNumbers="true" %}
```visual-basic
Public Sub ReadContents(ByVal filename As String)
```
{% endcode %}

This function used to directly print the file contents to the console. As we were trying to refine the API, this function was changed to contain an array of strings containing file lines. This caused `PrintContents` to be made, printing the contents to the console using `ReadContents`.

{% hint style="info" %}
Their method signatures are shown below.

{% code title="FileRead.cs" lineNumbers="true" %}
```csharp
public static string[] ReadContents(string filename)
public static void PrintContents(string filename)
public static void PrintContents(string filename, bool PrintLineNumbers, bool ForcePlain = false)
public static void DisplayInHex(long StartByte, long EndByte, byte[] FileByte)
```
{% endcode %}
{% endhint %}

### Removed `NotifyCreate()`

{% code title="Notifications.vb" lineNumbers="true" %}
```visual-basic
Public Function NotifyCreate(ByVal Title As String, ByVal Desc As String, ByVal Priority As NotifPriority, ByVal Type As NotifType) As Notification
```
{% endcode %}

This function was used to call the constructor of the Notification class and set the necessary variables to the new instance. It later was found out to be a syntactic sugar, so we removed it as the new constructor came.

{% hint style="info" %}
You can still create notifications using the constructor that its method signature is printed below.

{% code title="Notification.cs" lineNumbers="true" %}
```csharp
public Notification(string Title, string Desc, NotificationManager.NotifPriority Priority, NotificationManager.NotifType Type)
```
{% endcode %}
{% endhint %}

### **Split the theme-related tools from `ColorTools`**

{% code title="ColorTools.vb" lineNumbers="true" %}
```visual-basic
Public colorTemplates As New Dictionary(Of String, ThemeInfo)
Public Sub ApplyThemeFromResources(ByVal theme As String)
Public Sub ApplyThemeFromFile(ByVal ThemeFile As String)
Public Function SetColors(ThemeInfo As ThemeInfo) As Boolean
```
{% endcode %}

These theme-related functions were found at the `ColorTools` module, but they could have been separated from the main topic, so we decided to make an entirely new class dedicated to the theme tools.

As a result, `colorTemplates` was renamed to Themes and the theme overload for `SetColors` was renamed to `SetColorsTheme`.

{% hint style="info" %}
In the current API revision, you can apply your theme using the following functions:

{% code title="ThemeTools.cs" lineNumbers="true" %}
```csharp
public static void ApplyThemeFromResources(string theme)
public static void ApplyThemeFromFile(string ThemeFile)
public static void SetColorsTheme(ThemeInfo ThemeInfo)
```
{% endcode %}
{% endhint %}

### **Implemented help helpers for commands and arguments**

{% code title="CommandInfo.vb" lineNumbers="true" %}
```visual-basic
Public Sub New(ByVal Command As String, ByVal Type As ShellCommandType, ByVal HelpDefinition As String, ByVal ArgumentsRequired As Boolean, ByVal MinimumArguments As Integer, Optional Strict As Boolean = False, Optional Wrappable As Boolean = False, Optional NoMaintenance As Boolean = False, Optional Obsolete As Boolean = False, Optional SettingVariable As Boolean = False)
```
{% endcode %}

The commands used to use the hard-coded extra help to provide additional information about the command usage. However, we needed to make this more extensible, so we decided to implement the help helpers. You need to change how you call the constructor to support the help helpers.

{% hint style="info" %}
To implement the help helpers, the constructor of the `CommandInfo` class has been changed to hold the base command, which already holds a command interface containing the overridable `HelpHelper()` method.

```csharp
// CommandInfo.cs
public CommandInfo(string Command, ShellType Type, string HelpDefinition, CommandArgumentInfo CommandArgumentInfo, BaseCommand CommandBase, CommandFlags Flags = CommandFlags.None)
public CommandInfo(string Command, string Type, string HelpDefinition, CommandArgumentInfo CommandArgumentInfo, BaseCommand CommandBase, CommandFlags Flags = CommandFlags.None)

// BaseCommand.cs
public virtual void HelpHelper()
```
{% endhint %}

### **Enumerized the reasons for the three events**

{% code title="Events.vb" lineNumbers="true" %}
```visual-basic
Public Event LoginError(ByVal Username As String, ByVal Reason As String)
Public Event ThemeSetError(ByVal Theme As String, ByVal Reason As String)
Public Event ColorSetError(ByVal Reason As String)
```
{% endcode %}

These events used to hold the reason string. Since it could be anything, we've decided to make an enumeration out of it based on the error type.

{% hint style="info" %}
The event system has been heavily redesigned in the latest API so that you can use the event enumeration with parameters.

* `EventType.LoginError` (Username, Reason)
* `EventType.ThemeSetError` (Theme, Reason)
* `EventType.ColorSetError` (Reason)

{% code title="EventsManager.cs" lineNumbers="true" %}
```csharp
public static void FireEvent(EventType Event, params object[] Params)
```
{% endcode %}
{% endhint %}

### **Split the custom screensaver code**

{% code title="Screensaver.vb" lineNumbers="true" %}
```visual-basic
Public Sub CompileCustom(ByVal file As String)
Function GenSaver(ByVal PLang As String, ByVal code As String) As ICustomSaver
Public Sub InitializeCustomSaverSettings()
Public Sub SaveCustomSaverSettings()
Public Sub AddCustomSaverToSettings(ByVal CustomSaver As String)
Public Sub RemoveCustomSaverFromSettings(ByVal CustomSaver As String)
Public Function GetCustomSaverSettings(ByVal CustomSaver As String, ByVal SaverSetting As String) As Object
Public Function SetCustomSaverSettings(ByVal CustomSaver As String, ByVal SaverSetting As String, ByVal Value As Object) As Boolean
```
{% endcode %}

The custom screensaver code were located alongside the screensaver management code. They're relocated to the new location based on the type:

* `CompileCustom` and `GenSaver` functions were moved to `CustomSaverCompiler`
* The remaining six functions were moved to `CustomSaverTools`

{% hint style="info" %}
The custom screensaver compilation functions were remade as we've migrated to DLL-only modding and screensaver code.
{% endhint %}

### **Moved few variables regarding mods**

{% code lineNumbers="true" %}
```visual-basic
'ModParser.vb
Public Interface IScript
Public scripts As New Dictionary(Of String, ModInfo)

'HelpSystem.vb
Public moddefs As New Dictionary(Of String, String)
```
{% endcode %}

These variables were used to manage your mods. However, the following changes occurred:

* All mod definitions, like `ModDefs`, were moved to `HelpSystem`.
* Scripts dictionary was moved to `ModManager`
* `IScript` was moved from `ModParser` to its individual file

{% hint style="info" %}
The variables were remade so they're now secure. The `IScript` interface is essential for your mods.
{% endhint %}

### **Cleaned some flags up**

{% code title="Flags.vb" lineNumbers="true" %}
```visual-basic
Public StopPanicAndGoToDoublePanic As Boolean
Public instanceChecked As Boolean
```
{% endcode %}

These variables were accidentally exposed to the public API, so we decided to make them internally available so mods can't assign values to them.

### **Unified the overloads for writing functions**

The `W()` function, their `C` and `C16` siblings, and their sister functions (such as `WriteSlowlyC()`, etc.) were made to separate the overloads for color level support. However, they've been unified to one master function containing overloads for each color level.

Also, these functions had their one-letter functions changed to Write so that the one-letter function names were no longer confusing, though we had to choose that because the codebase was using Visual Basic that imported the `Microsoft.VisualBasic.FileSystem` module that contained the Write function. Their documentation are still available below:

{% embed url="https://learn.microsoft.com/en-us/dotnet/api/microsoft.visualbasic.filesystem?view=netframework-4.8" %}

{% embed url="https://learn.microsoft.com/en-us/dotnet/api/microsoft.visualbasic.filesystem.write?view=netframework-4.8" %}

These caused many problems to the point that we needed to edit many source files to try to bypass the `FileSystem` module.

{% hint style="warning" %}
Your mods might break if any of them uses the console writing functions from KS, so change all the `W()` instances to `Write()` and remove any "`C`" or "`C16`" suffixes.
{% endhint %}

{% hint style="warning" %}
When writing such functions, you'll discover that the arguments parsing is stricter than the previous, due to how we've implemented the message argument. Make explicit casts to get the same behavior as the previous versions.
{% endhint %}

### **Actually removed `AliasType`**

{% code title="AliasManager.vb" lineNumbers="true" %}
```visual-basic
Public Enum AliasType
```
{% endcode %}

This enumeration used to host all the shell types, but it was later found out that `ShellCommandType` came earlier, and it was basically a carbon-copy of the enumeration, so we decided to remove it.

{% hint style="danger" %}
We advice you to cease using this function.
{% endhint %}

### **Reworked on the fetch kernel update API**

{% code title="KernelTools.vb" lineNumbers="true" %}
```visual-basic
Public Function FetchKernelUpdates() As List(Of String)
```
{% endcode %}

This function used to return a list of kernel updates, but it was later found out that securing it with a class that holds the kernel update information was better, so we changed the return type of the function to KernelUpdate.

{% hint style="info" %}
The `FetchKernelUpdates` is still available as a usable method that mods can use by referring to the method signature below:

{% code title="UpdateManager.cs" lineNumbers="true" %}
```csharp
public static KernelUpdate FetchKernelUpdates()
```
{% endcode %}
{% endhint %}

### **Removed the RGB class**

{% code title="RGB.vb" lineNumbers="true" %}
```visual-basic
Public Class RGB
```
{% endcode %}

This class used to hold the red, green, and blue variables, but it was later found out that the `Color` class provides the same functionality, so we decided to remove the class.

{% hint style="info" %}
The `Color` class is available in the Terminaux library that Nitrocid KS uses.
{% endhint %}

### **Increased security of the "scripts" variable**

{% code title="ModParser.vb" lineNumbers="true" %}
```visual-basic
Public scripts As New Dictionary(Of String, ModInfo)
```
{% endcode %}

This variable used to store the mod information for each loaded mod. It was found that mods can access this variable and perform illegal operations, so we decided to internalize it.

{% hint style="info" %}
If you really want to list the mods using this dictionary, consider using the function for it. Its method signature is shown below:

{% code title="ModManager.cs" lineNumbers="true" %}
```csharp
public static Dictionary<string, ModInfo> ListMods()
public static Dictionary<string, ModInfo> ListMods(string SearchTerm)
```
{% endcode %}
{% endhint %}

### **`[G|S]etConfig*` functions and subs are now obsolete**

{% code title="" lineNumbers="true" %}
```visual-basic
Public Sub SetConfigValue(ByVal Variable As String, ByVal VariableValue As Object)
Public Function GetConfigValue(ByVal Variable As String) As Object
```
{% endcode %}

These functions were exclusively used by the settings applications to set and get the configuration values. They were derivatives of the already existing `SetValue`, `GetValue`, and `GetPropertyValueInVariable` functions with slight changes, so we removed the two above functions to avoid code duplicates.

{% hint style="info" %}
The three functions still exist, but relocated in the `FieldManager` and `PropertyManager` classes.
{% endhint %}

### **Made `IShell` and shell stacks to handle shells**

{% code lineNumbers="true" %}
```visual-basic
'All common shell files and InitializeShell() functions and their derivatives were removed.
Sub InitTShell()
Public Sub InitializeShell()
Public Sub SFTPInitiateShell(Optional ByVal Connects As Boolean = False, Optional ByVal Address As String = "")
Public Sub InitiateShell(Optional ByVal Connects As Boolean = False, Optional ByVal Address As String = "")
Public Sub InitiateRSSShell(Optional FeedUrl As String = "")
Sub OpenMailShell(Address As String)
Public Sub InitializeZipShell(ByVal ZipFile As String)
Public Sub InitializeTextShell(ByVal FilePath As String)
```
{% endcode %}

As part of the shell rewrite, we decided to make `IShell` and shell stacks to handle the shells. This caused us to remove all `InitializeShell()` functions shown above to reduce confusion.

{% hint style="info" %}
To implement your shell in your mod, use the `IShell` interface.
{% endhint %}

### **Cleaned up `GetLine()` so strcommand is first**

{% code title="Shell.vb" lineNumbers="true" %}
```visual-basic
Public Sub GetLine(ByVal ArgsMode As Boolean, ByVal strcommand As String, Optional ByVal IsInvokedByKernelArgument As Boolean = False, Optional ByVal OutputPath As String = "")
```
{% endcode %}

`GetLine()` was used to parse the given command input. However, it needed cleanup because `ArgsMode` was only used for one purpose, and it felt so unnecessary that it shouldn't have been implemented in the first place, so we decided to remove it.

{% hint style="info" %}
`GetLine` has been massively changed so that it actually gets the input and executes a given command in your shell. You can use this function in your shell to listen for commands. The method signatures show the ways of how you can use this routine.

{% code title="Shell.cs" lineNumbers="true" %}
```csharp
public static void GetLine()
public static void GetLine(string FullCommand)
public static void GetLine(string FullCommand, string OutputPath = "")
public static void GetLine(string FullCommand, string OutputPath = "", ShellType ShellType = ShellType.Shell)
public static void GetLine(string FullCommand, string OutputPath = "", string ShellType = "Shell", bool restoreDriver = true)
```
{% endcode %}
{% endhint %}

### **Renamed `ShellCommandType` to `ShellType`**

{% code title="CommandInfo.vb" lineNumbers="true" %}
```visual-basic
Public Enum ShellCommandType
```
{% endcode %}

This enumeration was used to indicate the shell type to perform the operation related to shell. The enumeration was used for shell purposes other than the commands, so we decided to rename it to ShellType for easier writing.

{% hint style="info" %}
For built-in shells, you can use the `ShellType` enumeration in functions that take it. However, when defining custom shells, be sure to register your shell with `ShellTypeManager` using the `RegisterShell()` function to tell KS that there is a new shell coming. Custom shells can't be used with the `ShellType` enumeration.

```csharp
// ShellType.cs
public enum ShellType

// ShellTypeManager.cs
public static void RegisterShell(string ShellType, BaseShellInfo ShellTypeInfo)
```
{% endhint %}

### **Moved all the `GetLine()` functions for all shells to the master `GetLine()`**

{% code lineNumbers="true" %}
```visual-basic
Public Sub SFTPGetLine()
Public Sub FTPGetLine()
```
{% endcode %}

All the `GetLine()` functions were moved to the master GetLine as it has witnessed functional and cosmetic improvements. It now supports different shells.

{% hint style="info" %}
`GetLine()` has been massively changed so that it actually gets the input and executes a given command in your shell. You can use this function in your shell to listen for commands. The method signatures show the ways of how you can use this routine.

{% code title="Shell.cs" lineNumbers="true" %}
```csharp
public static void GetLine()
public static void GetLine(string FullCommand)
public static void GetLine(string FullCommand, string OutputPath = "")
public static void GetLine(string FullCommand, string OutputPath = "", ShellType ShellType = ShellType.Shell)
public static void GetLine(string FullCommand, string OutputPath = "", string ShellType = "Shell", bool restoreDriver = true)
```
{% endcode %}
{% endhint %}

### **Moved `GetTerminalEmulator()` to `ConsoleExtensions`**

{% code title="KernelTools.vb" lineNumbers="true" %}
```visual-basic
Public Function GetTerminalEmulator() As String
```
{% endcode %}

This function was used to check the sanity of the terminal emulator for Linux systems. It was later found out that it could have been relocated to `ConsoleExtensions` for easier calling.

{% hint style="info" %}
`KernelPlatform` now hosts this function, but the method signature is the same.

{% code title="KernelPlatform.cs" lineNumbers="true" %}
```csharp
public static string GetTerminalEmulator()
```
{% endcode %}
{% endhint %}

### **Split the exceptions to separate codefiles**

{% code title="Exceptions.vb" lineNumbers="true" %}
```visual-basic
Public Class Exceptions
    Public Class NullUsersException
    Public Class AliasInvalidOperationException
(...)
```
{% endcode %}

These exceptions used to be hosted on the Exceptions masterclass, but they were separated for easier access.

{% hint style="info" %}
The kernel exception system had a massive rewrite to the point where every single kernel exception was given an enumeration value and their own message. You can throw these exceptions using the `KernelException` masterclass.

{% code title="KernelException.cs" lineNumbers="true" %}
```csharp
public KernelException(KernelExceptionType exceptionType)
public KernelException(KernelExceptionType exceptionType, Exception e)
public KernelException(KernelExceptionType exceptionType, string message)
public KernelException(KernelExceptionType exceptionType, string message, params object[] vars)
public KernelException(KernelExceptionType exceptionType, string message, Exception e)
public KernelException(KernelExceptionType exceptionType, string message, Exception e, params object[] vars)
```
{% endcode %}
{% endhint %}

### **Renamed new line field to `NewLine` from `vbNewLine`**

{% code title="Kernel.vb" lineNumbers="true" %}
```visual-basic
Public ReadOnly vbNewLine As String
```
{% endcode %}

`vbNewLine` sounded like it came from Visual Basic 6.0 (not .NET), a COM-based Windows-only language which we'll never support, and because of the below namespace changes that causes `Microsoft.VisualBasic` namespace to break things related to `vbNewLine`, we decided to change it to just `NewLine`.

### **Namespaced the entire codebase**

To further organize the codebase, we decided to namespace each one of them based on the folders in the source code. This way, we'd have the following namespaces:

* `KS.Arguments`
* `KS.Arguments.ArgumentBase`
* `KS.Arguments.CommandLineArguments`
* `KS.Arguments.KernelArguments`
* `KS.Arguments.PreBootCommandLineArguments`
* `KS.ConsoleBase`
* `KS.ConsoleBase.Themes`
* `KS.ConsoleBase.Themes.Studio`
* `KS.Files`
* `KS.Hardware`
* `KS.Kernel`
* `KS.Kernel.Exceptions`
* `KS.Languages`
* `KS.Login`
* `KS.ManPages`
* `KS.Misc`
* `KS.Misc.Beautifiers`
* `KS.Misc.Calendar`
* `KS.Misc.Calendar.Events`
* `KS.Misc.Calendar.Reminders`
* `KS.Misc.Configuration`
* `KS.Misc.Encryption`
* `KS.Misc.Execution`
* `KS.Misc.Forecast`
* `KS.Misc.Games`
* `KS.Misc.JsonShell`
* `KS.Misc.JsonShell.Commands`
* `KS.Misc.Notifiers`
* `KS.Misc.Platform`
* `KS.Misc.Probers`
* `KS.Misc.Reflection`
* `KS.Misc.Screensaver`
* `KS.Misc.Screensaver.Customized`
* `KS.Misc.Screensaver.Displays`
* `KS.Misc.Splash`
* `KS.Misc.Splash.Splashes`
* `KS.Misc.TextEdit`
* `KS.Misc.TextEdit.Commands`
* `KS.Misc.Threading`
* `KS.Misc.Timers`
* `KS.Misc.Writers`
* `KS.Misc.Writers.ConsoleWriters`
* `KS.Misc.Writers.DebugWriters`
* `KS.Misc.Writers.FancyWriters`
* `KS.Misc.Writers.FancyWriters.Tools`
* `KS.Misc.Writers.MiscWriters`
* `KS.Misc.ZipFile`
* `KS.Misc.ZipFile.Commands`
* `KS.Modifications`
* `KS.Network`
* `KS.Network.FTP`
* `KS.Network.FTP.Commands`
* `KS.Network.FTP.Filesystem`
* `KS.Network.FTP.Transfer`
* `KS.Network.HTTP`
* `KS.Network.HTTP.Commands`
* `KS.Network.Mail`
* `KS.Network.Mail.Commands`
* `KS.Network.Mail.Directory`
* `KS.Network.Mail.PGP`
* `KS.Network.Mail.Transfer`
* `KS.Network.RemoteDebug`
* `KS.Network.RemoteDebug.Commands`
* `KS.Network.RemoteDebug.Interface`
* `KS.Network.RPC`
* `KS.Network.RSS`
* `KS.Network.RSS.Commands`
* `KS.Network.RSS.Instance`
* `KS.Network.SFTP`
* `KS.Network.SFTP.Commands`
* `KS.Network.SFTP.Filesystem`
* `KS.Network.SFTP.Transfer`
* `KS.Network.SSH`
* `KS.Scripting`
* `KS.Scripting.Interaction`
* `KS.Shell`
* `KS.Shell.Commands`
* `KS.Shell.ShellBase`
* `KS.Shell.Shells`
* `KS.TestShell`
* `KS.TestShell.Commands`
* `KS.TimeDate`

{% hint style="info" %}
New namespaces get created and/or changed each major release of the kernel, so the list above is only relevant at the time the change was committed. The API reference will always display all the available namespaces.
{% endhint %}

### **Removed built-in string evaluators**

{% code title="StringEvaluators.vb" lineNumbers="true" %}
```visual-basic
Public Function Evaluate(ByVal Var As String) As Object
Public Function EvaluateFast(ByVal Var As String, ByVal VarType As Type) As Object
```
{% endcode %}

The built-in string evaluators were used for the calculator functionality in the kernel, but it was later found out that it was too slow and insecure. We decided to remove these evaluators as a result.

{% hint style="danger" %}
We advice you to cease using this function.
{% endhint %}

## From 0.0.20 to 0.0.21

This version was a minor update to 0.0.20.0.

{% content-ref url="../version-release-notes/v0.0.21.x-series.md" %}
[v0.0.21.x-series.md](../version-release-notes/v0.0.21.x-series.md)
{% endcontent-ref %}

### **Consolidated the obsolete functions**

{% code title="SettingsApp.vb" lineNumbers="true" %}
```visual-basic
Public Function FindSetting(Pattern As String, Screensaver As Boolean) As List(Of String)

<Obsolete("Use SetValue(String, Object) instead.")>
Public Sub SetConfigValueField(Variable As String, VariableValue As Object)

<Obsolete("Use GetValue(String) instead.")>
Public Function GetConfigValueField(Variable As String) As Object

<Obsolete("Use GetPropertyValueInVariable(String, String) instead.")>
Public Function GetConfigPropertyValueInVariableField(Variable As String, [Property] As String) As Object
```
{% endcode %}

These obsolete functions were used by the settings app to do the following:

* `FindSetting(String, Boolean)` was used to return the found settings, but the boolean variable indicates if the app is going to use the screensaver settings metadata.
* `SetConfigValueField(String, Object)` was used to be a wrapper to the `SetValue(String, Object)` function
* `GetConfigValueField(String)` was used to be a wrapper to the `GetValue(String)` function
* `GetConfigPropertyValueInVariableField(String, String)` was used to be a wrapper to the `GetPropertyValueInVariable(String, String)` function

They're removed as a result of the migration of these functions.

{% hint style="info" %}
`FindSetting()` was moved to `ConfigTools`, and it was improved. You can see the method signature below.

{% code title="ConfigTools.cs" lineNumbers="true" %}
```csharp
public static List<InputChoiceInfo> FindSetting(string Pattern, JToken SettingsToken)
```
{% endcode %}
{% endhint %}

## From 0.0.21 to 0.0.22

This version was a great update to the API v2.0 series, because it added Android support to KS!

{% content-ref url="../version-release-notes/v0.0.22.x-series.md" %}
[v0.0.22.x-series.md](../version-release-notes/v0.0.22.x-series.md)
{% endcontent-ref %}

### **Separated properties code to `PropertyManager`**

{% code title="FieldManager.vb" lineNumbers="true" %}
```visual-basic
Public Function GetPropertyValueInVariable(Variable As String, [Property] As String) As Object
Public Function GetPropertyValueInVariable(Variable As String, [Property] As String, VariableType As Type) As Object
Public Function GetProperties(VariableType As Type) As Dictionary(Of String, Object)
Public Function GetPropertiesNoEvaluation(VariableType As Type) As Dictionary(Of String, Type)
```
{% endcode %}

These functions were used to get the property values and properties themselves. However, it was found that they're located on `FieldManager` and we felt that it was a bit misleading, so we decided to move them to their own dedicated class, `PropertyManager`.

{% hint style="info" %}
These functions have been renamed to shorter names and used cached expressions to slightly improve performance. You can find their method signatures.

{% code title="PropertyManager.cs" lineNumbers="true" %}
```csharp
public static object GetPropertyValue(string Variable)
public static object GetPropertyValue(string Variable, Type VariableType)
public static Dictionary<string, object> GetProperties(Type VariableType)
public static Dictionary<string, Type> GetPropertiesNoEvaluation(Type VariableType)
```
{% endcode %}
{% endhint %}

### **Events and reminders format**

Events and reminders file formats have been changed from binary file to XML files as `BinarySerializer` was being deprecated because of it being vulnerable to attacks as describes in the below documentation link:

{% embed url="https://learn.microsoft.com/en-us/dotnet/standard/serialization/binaryformatter-security-guide#binaryformatter-security-vulnerabilities" %}

### **Deprecation of `IScript.PerformCmd()`**

{% code title="IScript.vb" lineNumbers="true" %}
```visual-basic
Sub PerformCmd(Command As CommandInfo, Optional Args As String = "")
```
{% endcode %}

As we implemented the fully-fledged `CommandBase.Execute()` function which does the same thing as `IScript.PerformCmd()`, we've deprecated the function in the interface to take advantage of the `CommandBase.Execute()` routine.

{% hint style="info" %}
`BaseCommand.Execute()` can be overridden in the below method signature:

{% code title="BaseCommand.cs" lineNumbers="true" %}
```csharp
public virtual void Execute(string StringArgs, string[] ListArgsOnly, string[] ListSwitchesOnly)
```
{% endcode %}
{% endhint %}

### **Removed `ReadLineLong()`**

{% code title="Input.vb" lineNumbers="true" %}
```visual-basic
Public Function ReadLineLong() As String
```
{% endcode %}

This function was implemented to take advantage of the long input support in the built-in .NET console reader. As ReadLine.Reboot was used, this function has been removed.

{% hint style="info" %}
Long inputs are supported by the `Input.ReadLine` function that has several method signatures shown below:

{% code title="Input.cs" lineNumbers="true" %}
```csharp
public static string ReadLine()
public static string ReadLine(bool UseCtrlCAsInput)
public static string ReadLine(string InputText, string DefaultValue)
public static string ReadLine(string InputText, string DefaultValue, bool UseCtrlCAsInput)
```
{% endcode %}
{% endhint %}

## From 0.0.22 to 0.0.23

This version was the last version from the API v2.0 series.

{% content-ref url="../version-release-notes/v0.0.23.x-series.md" %}
[v0.0.23.x-series.md](../version-release-notes/v0.0.23.x-series.md)
{% endcontent-ref %}

### **Deprecation of `ICustomSaver`**

As we've implemented `BaseScreensaver` to better handle screensavers, we decided to deprecate `ICustomSaver` in favor of the new screensaver model. This would merge all kernel threads of individual screensavers to one master screensaver thread.

{% hint style="info" %}
All new screensavers should use the `BaseScreensaver` class. All existing screensavers should migrate from `ICustomSaver` to `BaseScreensaver`.
{% endhint %}
