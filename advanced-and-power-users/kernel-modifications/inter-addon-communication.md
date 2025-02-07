---
description: Mods talking to an addon or more than one addon in Nitrocid
---

# 📞 Inter-Addon Communication

Inter-Addon Communication allows your mods to execute the publicly-available functions of a kernel addon. It allows your mods to talk to the kernel addons in a mechanism that doesn't interfere with the other end's operations.

{% hint style="info" %}
When getting the name of an addon, you can consult the `GetAddonName()` function, indicating what addon you want to use, such as `KnownAddons.ExtrasAmusements` for `Extras - Amusements`.
{% endhint %}

### How to call a publicly accessible addon function?

To execute custom addon functions in your mod, you must specify the full addon name, such as `Extras - RSS Shell`, and the function to execute in the `ExecuteCustomAddonFunction()` method:

```csharp
public static object ExecuteCustomAddonFunction(KnownAddons addonName, string functionName)
public static object ExecuteCustomAddonFunction(KnownAddons addonName, string functionName, params object[] parameters)
public static object ExecuteCustomAddonFunction(string addonName, string functionName)
public static object ExecuteCustomAddonFunction(string addonName, string functionName, params object[] parameters)
```

{% hint style="info" %}
You must specify the main addon name in the above function, since it uses that name to query an addon for available functions. Remember, you can use the `KnownAddons` enumeration to simplify things!

`ExecuteCustomAddonFunction()` returns `null` under the following conditions:

* There are no functions in all the mod parts from your mod.
* There is a function, but the delegate is unspecified.

It throws an exception if these conditions are true:

* There is no function that goes by the name of the specified function name that you plan to execute.
{% endhint %}

### How can I get/set a value from/to a property or a field?

To get a property value or a field value from an addon, you can call the following functions:

```csharp
public static object GetCustomAddonPropertyValue(KnownAddons addonName, string propertyName)
public static object GetCustomAddonFieldValue(KnownAddons addonName, string fieldName)
public static object GetCustomAddonPropertyValue(string addonName, string propertyName)
public static object GetCustomAddonFieldValue(string addonName, string fieldName)
```

Similarly, to set a property value or a field value declared publicly by an addon, you can call the following functions:

```csharp
public static void SetCustomAddonPropertyValue(KnownAddons addonName, string propertyName, object value)
public static void SetCustomAddonFieldValue(KnownAddons addonName, string fieldName, object value)
public static void SetCustomAddonPropertyValue(string addonName, string propertyName, object value)
public static void SetCustomAddonFieldValue(string addonName, string fieldName, object value)
```

{% hint style="info" %}
You must specify the main addon name in the above function, since it uses that name to query an addon for available properties or fields. Remember, you can use the `KnownAddons` enumeration to simplify things!

`GetCustomModPropertyValue()` and `GetCustomModFieldValue()` return `null` under the following conditions:

* There are no properties or fields in all the mod parts from your mod.
* There is a property or field, but the delegate is unspecified.

It throws an exception if these conditions are true:

* There is no property or field that goes by the name of the specified name that you plan to execute.
{% endhint %}

### Listing functions, properties, and fields

You can now list all the available functions, properties, and fields from a specific mod using one of the following functions:

{% code title="InterAddonTools.cs" lineNumbers="true" %}
```csharp
public static string[] ListAvailableFunctions(string addonName)
public static string[] ListAvailableProperties(string addonName)
public static string[] ListAvailableFields(string addonName)
```
{% endcode %}

{% hint style="info" %}
You must specify the main mod name in the above functions, since they use that name to fetch all mod parts and query them for available functions, fields, or properties.

The three functions return an empty array under the following conditions:

* There are no properties or fields in all the mod parts from your mod.
{% endhint %}

### Listing function and set property parameters

You can list all the function and set property parameters in depth by using the following functions:

```csharp
public static ParameterInfo[] GetFunctionParameters(string addonName, string functionName)
public static ParameterInfo[] GetSetPropertyParameters(string addonName, string propertyName)
```

## Supported addons

Items that are marked in green, yellow, and red resemble functions, properties, and fields respectively. The following addons support inter-addon communication:

* Nitrocid.Extras.ArchiveShell
  * <mark style="color:green;">`ListArchiveEntries`</mark>
  * <mark style="color:green;">`ExtractFileEntry`</mark>
  * <mark style="color:green;">`PackFile`</mark>
  * <mark style="color:green;">`ChangeWorkingArchiveDirectory`</mark>
  * <mark style="color:green;">`ChangeWorkingArchiveLocalDirectory`</mark>
* Nitrocid.Extras.Calendar
  * <mark style="color:green;">`PrintCalendar`</mark>
  * <mark style="color:green;">`PrintCalendar2`</mark>
  * <mark style="color:green;">`PrintCalendar3`</mark>
  * <mark style="color:green;">`AddEvent`</mark>
  * <mark style="color:green;">`RemoveEvent`</mark>
  * <mark style="color:green;">`ListEvents`</mark>
  * <mark style="color:green;">`LoadEvents`</mark>
  * <mark style="color:green;">`LoadEvent`</mark>
  * <mark style="color:green;">`SaveEvents`</mark>
  * <mark style="color:green;">`SaveEvents2`</mark>
  * <mark style="color:green;">`SaveEvent`</mark>
  * <mark style="color:green;">`SaveEvent2`</mark>
  * <mark style="color:green;">`AddReminder`</mark>
  * <mark style="color:green;">`RemoveReminder`</mark>
  * <mark style="color:green;">`ListReminders`</mark>
  * <mark style="color:green;">`LoadReminders`</mark>
  * <mark style="color:green;">`LoadReminder`</mark>
  * <mark style="color:green;">`SaveReminders`</mark>
  * <mark style="color:green;">`SaveReminders2`</mark>
  * <mark style="color:green;">`SaveReminder`</mark>
  * <mark style="color:green;">`SaveReminder2`</mark>
* Nitrocid.Extras.Contacts
  * <mark style="color:green;">`GetContacts`</mark>
  * <mark style="color:green;">`ImportContacts`</mark>
  * <mark style="color:green;">`InstallContacts`</mark>
  * <mark style="color:green;">`InstallContacts2`</mark>
  * <mark style="color:green;">`InstallContactFromMeCard`</mark>
  * <mark style="color:green;">`RemoveContact`</mark>
  * <mark style="color:green;">`RemoveContacts`</mark>
  * <mark style="color:green;">`GetContact`</mark>
  * <mark style="color:green;">`SearchNext`</mark>
  * <mark style="color:green;">`SearchNext2`</mark>
  * <mark style="color:green;">`SearchPrevious`</mark>
  * <mark style="color:green;">`SearchPrevious2`</mark>
* Nitrocid.Extras.Diagnostics
  * <mark style="color:green;">`GetThreadBacktraces`</mark>
* Nitrocid.Extras.Docking
  * <mark style="color:green;">`DockScreen`</mark>
  * <mark style="color:green;">`DockScreen2`</mark>
  * <mark style="color:green;">`DoesDockScreenExist`</mark>
  * <mark style="color:green;">`GetDockScreenNames`</mark>
  * <mark style="color:green;">`GetDockScreens`</mark>
* Nitrocid.Extras.Forecast
  * <mark style="color:green;">`GetWeatherInfo`</mark>
  * <mark style="color:green;">`GetWeatherInfo2`</mark>
  * <mark style="color:green;">`PrintWeatherInfo`</mark>
  * <mark style="color:green;">`PrintWeatherInfo2`</mark>
  * <mark style="color:green;">`GetWeatherInfoOwm`</mark>
  * <mark style="color:green;">`GetWeatherInfoOwm2`</mark>
  * <mark style="color:green;">`GetWeatherInfoOwm3`</mark>
  * <mark style="color:green;">`GetWeatherInfoOwm4`</mark>
  * <mark style="color:green;">`PrintWeatherInfoOwm`</mark>
  * <mark style="color:green;">`PrintWeatherInfoOwm2`</mark>
  * <mark style="color:yellow;">`PreferredUnit`</mark>
* Nitrocid.Extras.FtpShell
  * <mark style="color:green;">`FTPListRemote`</mark>
  * <mark style="color:green;">`FTPListRemote2`</mark>
  * <mark style="color:green;">`FTPDeleteRemote`</mark>
  * <mark style="color:green;">`FTPChangeRemoteDir`</mark>
  * <mark style="color:green;">`FTPMoveItem`</mark>
  * <mark style="color:green;">`FTPCopyItem`</mark>
  * <mark style="color:green;">`FTPChangePermissions`</mark>
  * <mark style="color:green;">`FTPGetHash`</mark>
  * <mark style="color:green;">`FTPGetHashes`</mark>
  * <mark style="color:green;">`FTPGetHashes2`</mark>
  * <mark style="color:green;">`FTPGetFile`</mark>
  * <mark style="color:green;">`FTPGetFile2`</mark>
  * <mark style="color:green;">`FTPGetFolder`</mark>
  * <mark style="color:green;">`FTPGetFolder2`</mark>
  * <mark style="color:green;">`FTPUploadFile`</mark>
  * <mark style="color:green;">`FTPUploadFile2`</mark>
  * <mark style="color:green;">`FTPUploadFolder`</mark>
  * <mark style="color:green;">`FTPUploadFolder2`</mark>
  * <mark style="color:green;">`FTPDownloadToString`</mark>
  * <mark style="color:green;">`PromptForPassword`</mark>
  * <mark style="color:green;">`TryToConnect`</mark>
  * <mark style="color:yellow;">`FileProgress`</mark>
  * <mark style="color:yellow;">`MultipleProgress`</mark>
* Nitrocid.Extras.HttpShell
  * <mark style="color:green;">`HttpDelete`</mark>
  * <mark style="color:green;">`HttpGetString`</mark>
  * <mark style="color:green;">`HttpGet`</mark>
  * <mark style="color:green;">`HttpPutString`</mark>
  * <mark style="color:green;">`HttpPutFile`</mark>
  * <mark style="color:green;">`HttpPostString`</mark>
  * <mark style="color:green;">`HttpPostFile`</mark>
  * <mark style="color:green;">`HttpAddHeader`</mark>
  * <mark style="color:green;">`HttpRemoveHeader`</mark>
  * <mark style="color:green;">`HttpListHeaders`</mark>
  * <mark style="color:green;">`HttpGetCurrentUserAgent`</mark>
  * <mark style="color:green;">`HttpSetUserAgent`</mark>
  * <mark style="color:green;">`NeutralizeUri`</mark>
* Nitrocid.Extras.JsonShell
  * <mark style="color:green;">`OpenJsonFile`</mark>
  * <mark style="color:green;">`CloseJsonFile`</mark>
  * <mark style="color:green;">`SaveFile`</mark>
  * <mark style="color:green;">`SaveFile2`</mark>
  * <mark style="color:green;">`WasJsonEdited`</mark>
  * <mark style="color:green;">`DetermineRootType`</mark>
  * <mark style="color:green;">`DetermineType`</mark>
  * <mark style="color:green;">`GetToken`</mark>
  * <mark style="color:green;">`GetTokenSafe`</mark>
  * <mark style="color:green;">`GetTokenSafe2`</mark>
  * <mark style="color:green;">`Add`</mark>
  * <mark style="color:green;">`Set`</mark>
  * <mark style="color:green;">`Remove`</mark>
  * <mark style="color:green;">`SerializeToString`</mark>
* Nitrocid.Extras.MailShell
  * <mark style="color:green;">`CreateMailDirectory`</mark>
  * <mark style="color:green;">`DeleteMailDirectory`</mark>
  * <mark style="color:green;">`RenameMailDirectory`</mark>
  * <mark style="color:green;">`OpenFolder`</mark>
  * <mark style="color:green;">`MailListDirectories`</mark>
  * <mark style="color:green;">`MailListMessages`</mark>
  * <mark style="color:green;">`MailListMessages2`</mark>
  * <mark style="color:green;">`MailRemoveMessage`</mark>
  * <mark style="color:green;">`MailRemoveAllBySender`</mark>
  * <mark style="color:green;">`MailMoveAllBySender`</mark>
  * <mark style="color:green;">`DecryptMessage`</mark>
  * <mark style="color:green;">`MailSendMessage`</mark>
  * <mark style="color:green;">`MailSendMessage2`</mark>
  * <mark style="color:green;">`MailSendEncryptedMessage`</mark>
  * <mark style="color:green;">`PopulateMessages`</mark>
  * <mark style="color:yellow;">`ShowPreview`</mark>
* Nitrocid.Extras.RssShell
  * <mark style="color:green;">`GetFirstArticle`</mark>
  * <mark style="color:green;">`SearchArticles`</mark>
  * <mark style="color:green;">`AddRSSFeedToBookmark`</mark>
  * <mark style="color:green;">`AddRSSFeedToBookmark2`</mark>
  * <mark style="color:green;">`RemoveRSSFeedFromBookmark`</mark>
  * <mark style="color:green;">`RemoveRSSFeedFromBookmark2`</mark>
  * <mark style="color:green;">`GetBookmarks`</mark>
  * <mark style="color:green;">`GetBookmark`</mark>
* Nitrocid.Extras.SftpShell
  * <mark style="color:green;">`SFTPListRemote`</mark>
  * <mark style="color:green;">`SFTPListRemote2`</mark>
  * <mark style="color:green;">`SFTPDeleteRemote`</mark>
  * <mark style="color:green;">`SFTPChangeRemoteDir`</mark>
  * <mark style="color:green;">`SFTPChangeLocalDir`</mark>
  * <mark style="color:green;">`SFTPGetCanonicalPath`</mark>
  * <mark style="color:green;">`SFTPGetFile`</mark>
  * <mark style="color:green;">`SFTPUploadFile`</mark>
  * <mark style="color:green;">`SFTPDownloadToString`</mark>
  * <mark style="color:green;">`PromptConnectionInfo`</mark>
  * <mark style="color:green;">`SFTPTryToConnect`</mark>
* Nitrocid.Extras.SqlShell
  * <mark style="color:green;">`IsSql`</mark>
  * <mark style="color:green;">`SqlEdit_OpenSqlFile`</mark>
  * <mark style="color:green;">`SqlEdit_CheckSqlFile`</mark>
  * <mark style="color:green;">`SqlEdit_SqlCommand`</mark>
* Nitrocid.Extras.Ssh
  * <mark style="color:green;">`PromptConnectionInfo`</mark>
  * <mark style="color:green;">`GetConnectionInfo`</mark>
  * <mark style="color:green;">`InitializeSSH`</mark>
  * <mark style="color:green;">`OpenShell`</mark>
  * <mark style="color:green;">`OpenShell2`</mark>
  * <mark style="color:green;">`OpenCommand`</mark>
  * <mark style="color:green;">`OpenCommand2`</mark>
* Nitrocid.Extras.ToDoList
  * <mark style="color:green;">`AddTask`</mark>
  * <mark style="color:green;">`RemoveTask`</mark>
  * <mark style="color:green;">`GetTask`</mark>
  * <mark style="color:green;">`GetTaskIndex`</mark>
  * <mark style="color:green;">`TaskExists`</mark>
  * <mark style="color:green;">`SetDone`</mark>
  * <mark style="color:green;">`SetUndone`</mark>
  * <mark style="color:green;">`GetTaskNames`</mark>
  * <mark style="color:green;">`SaveTasks`</mark>
  * <mark style="color:green;">`LoadTasks`</mark>
