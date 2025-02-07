---
description: Each of the settings contain their own format
icon: screwdriver
---

# Settings Format

This page specifies all the possible configuration formats.

## Kernel Configuration Entries

The most basic pseudo-representation of the settings entry file is printed below to illustrate the concept:

```json
[
    {
        "Name": "SectionName",
        "Desc": "Description of the section, usually a short one.",
        "DisplayAs": "Section name to be displayed in the app",
        "Keys": [
            {
                "Name": "Key name to be displayed in the app",
                "Type": "SType",
                "Variable": "VarName",
                "Description": "Description of the key, usually a short one."
            }
        ]
    }
]
```

Basically, all of the settings entry files start with an array containing one or more of the sections under the selected name for reference. Each section contains these necessary entries:

* `Name`: This variable holds the short section name.
  * The type of this variable is a **string**
* `Desc`: This variable holds the description of the section to be displayed in the bottom of the application.
  * The type of this variable is a **string**
  * The string is **localizable**
* `DisplayAs`: This variable holds the text to display in the settings application.
  * The type of this variable is a **string**
  * The string is **localizable**
* `Keys`: This variable holds the settings entries.
  * The type of this variable is an **array**

In the `Keys` variable, it contains an array of objects containing the settings entry information. They get used by the settings application to determine what to do when getting and setting the variable value. The entry information is detailed below (in general case):

* `Name`: This contains the key name to be displayed by the settings application
  * The type of this variable is a **string**
  * The string is **localizable**
* `Type`: The type of a variable, depending on the variable and on the purpose
  * The type of this variable is a **string**
* `Variable`: The variable to get and to set the values from/to
  * The type of this variable is a **string**
* `Description`: The description of the entry, to describe what is the purpose
  * The type of this variable is a **string**
  * The string is **localizable**

The variables that can be omitted are specified below:

* `IsInternal`: Specifies whether the variable is internal (can be omitted)
  * The type of this variable is a **boolean**
* `IsEnumerable`: If the variable is an enumerable that can be set, set to `true`
  * The type of this variable is a **boolean**
* `EnumerableIndex`: Required for `IsEnumerable` entries. Specifies the index starting from zero
  * The type of this variable is an **integer**
* `IsValuePath`: If the value takes a path, then `true`
  * The type of this variable is a **boolean**
* `IsPathCurrentPath`: If the value takes a relative path, then `true` to neutralize the input path to the current path
  * The type of this variable is a **boolean**
* `UnsupportedPlatforms`: The value is unsupported on one or more of the host platforms. The settings application won't display this value on an unsupported platform.
  * The type of this variable is an **array** of **strings**, and the possible values are: `["windows", "unix", "macos"]`
* `ValuePathType`: The path type to use for values. `IsPathCurrentPath` must be `false`.
  * The type of this variable is a **string**

In special cases, each type might require different variables to be set before the settings application knows what to do. These types are listed below:

* `SBoolean`
* `SInt`
* `SString`
* `SSelection`
* `SList`
* `SColor`
* `SChar`
* `SIntSlider`
* `SDouble`
* `SPreset`
* `SFiglet`

Only the types that require additional configuration are listed below. The below variables must be set to work with these types:

### `SSelection`

This type is a selection from the list of the values that a function or an enumeration returns.

#### Case 1

If the target listing is not an enumeration, the list can be obtained using a function that returns a list of possible options that can be set here: (`IsEnumeration` is `false`)

* `SelectionFunctionName`: The function within the below specified type that returns the list. It must contain no arguments.
  * The type of this variable is a **string**
* `SelectionFunctionType`: The type containing the function above
  * The type of this variable is a **string**
* `SelectionFallback`: An array of the fallback values in case the function fails
  * The type of this variable is an **array**

The format is as below:

```json
{
    "IsEnumeration": false,
    "SelectionFunctionName": "FunctionThatReturnsAList",
    "SelectionFunctionType": "TypeThatImplementsTheFunction",
    "SelectionFallback": [ "Fallback 1" ]
}
```

#### Case 2

If the target listing is an enumeration, the list can be obtained immediately with a list of possible options that can be set here: (`IsEnumeration` is `true`)

* `EnumerationInternal`: Specifies whether the enumeration is found within the kernel or within an external assembly
  * The type of this variable is a **boolean**
* `Enumeration`: The fully qualified name of the enumeration, including the whole namespace. If internal, `Nitrocid.` can be omitted. Enumerations inside the static classes must be appended with the plus sign, for example, `ConsoleBase.Inputs.Styles.ChoiceStyle+ChoiceOutputType`.
  * The type of this variable is a **string**
* `EnumerationAssembly`: An assembly name containing the enumeration. It can be omitted if enumeration is internal
  * The type of this variable is a **string**
* `EnumerationZeroBased`: If the enumeration is zero-based, then true. Otherwise, the enumeration is assumed to start from one
  * The type of this variable is a **boolean**

The format is as below:

```json
{
    "IsEnumeration": true,
    "EnumerationInternal": false,
    "Enumeration": "Namespace.Enum",
    "EnumerationAssembly": "Assembly",
    "EnumerationZeroBased": false
}
```

### `SList`

This type allows lists to be made in kernel settings. It contains list of configured values.

#### Case 1

The target list is always a parameterless function that always returns a list that is found under a Nitrocid KS type. The list type takes a string and splits it using the specified delimiter found in a variable.

* `SelectionFunctionName`: The function within the below specified type that returns the list. It must contain no arguments.
  * The type of this variable is a **string**
* `SelectionFunctionType`: The type containing the function above
  * The type of this variable is a **string**
* `DelimiterVariable`: The variable containing the delimiter character
  * The type of this variable is a **string**

The format is as below:

```json
{
    "SelectionFunctionName": "GetPathList",
    "SelectionFunctionType": "Filesystem",
    "DelimiterVariable": "PathLookupDelimiter"
}
```

#### Case 2

The target list is always a parameterless function that always returns a list that is found under a Nitrocid KS type. The list type takes a string and splits it using the specified delimiter.

* `SelectionFunctionName`: The function within the below specified type that returns the list. It must contain no arguments.
  * The type of this variable is a **string**
* `SelectionFunctionType`: The type containing the function above
  * The type of this variable is a **string**
* `Delimiter`: The delimiter character
  * The type of this variable is a **string**

The format is as below:

```json
{
    "SelectionFunctionName": "GetPathList",
    "SelectionFunctionType": "Filesystem",
    "Delimiter": ";"
}
```

### `SPreset`

This type is a shell preset that is treated as a string in the kernel configuration file, but it's actually set by the shell preset manager.

#### General case

It provides only one variable, `ShellType`, that specifies the shell type for listing all the presets. It delegates the listing functions to `PromptPresetManager.PromptForPresets()` function with the shell type in the `Nitrocid.Shell.Prompts` namespace.

* `SelectionFunctionName`: The function within the below specified type that returns the list. It must contain no arguments.
  * The type of this variable is a **string**

The format is as below:

```json
{
    "ShellType": "Shell"
}
```

### `SIntSlider`

This type is for integer values which are limited. They're surrounded between the minimum number and the maximum number, which can be represented like this (`x` is a value):

$$
N_{min} < x < N_{max}
$$

#### General case

There are two variables that must be set. One for the minimum value and one for the maximum value.

* `MinimumValue`: The minimum value to be set
  * The type of this variable is an **integer**
* `MaximumValue`: The maximum value to be set
  * The type of this variable is an **integer**

The format is as below:

```json
{
    "MinimumValue": 0,
    "MaximumValue": 10
}
```

## User Configuration

The kernel makes a user configuration entry for each new user created either by the `adduser` command or by the call of the `UserManagement.InitializeUser()` function in the `Nitrocid.Users` namespace under the name of `Users.json` found in the kernel configuration directory. The format of the file looks like this:

```json
[
  {
    "Username": "name",
    "FullName": "Full Name",
    "PreferredLanguage": "",
    "Groups": [],
    "Password": "encrypted password",
    "Flags": 1,
    "Permissions": [],
    "CustomSettings": {
      "key1": [
        "value1"
      ],
      (...)
    }
  }
]
```

Let's explain each key one by one:

* `Username`: Stores the username
  * The type of this variable is a **string**
* `FullName`: Stores the full name
  * The type of this variable is a **string**
* `PreferredLanguage`: Stores the preferred user language in three-letter language name format. Can be left blank to use the kernel-wide language
  * The type of this variable is a **string**
* `Groups`: What groups the user is in?
  * The type of this variable is an **array** of **string**
* `Password`: Stores the password
  * The type of this variable is a **string** encoded with the SHA256 hash
* `Flags`: Specifies a number representing the flags
  * The type of this variable is an **integer** that can be a sum of the following flags:
    * `1`: Administrator
    * `2`: Anonymous
    * `4`: Disabled
* `Permissions`: Stores the array of permissions found under [the permission list, `PermissionTypes`](https://github.com/Aptivi/Kernel-Simulator/blob/master/public/Kernel%20Simulator/Users/Permissions/PermissionTypes.cs).
  * The type of this variable is an **array** of **string**
* `CustomSettings`: Specifies the customized settings entries
  * The type of this variable is an **array** of **properties that their values represent an array of strings**

## Group configuration

The kernel also supports user groups created either by the `addgroup` command or by the call of the `GroupManagement.AddGroup()` function in the `Nitrocid.Users.Groups` namespace under the name of `UserGroups.json` found in the kernel configuration directory. The format of the file looks like this:

```json
[
  {
    "GroupName": "testGroup",
    "Permissions": []
  }
]
```

Let's explain each key one by one:

* `GroupName`: The name of the group
  * The type of this variable is a **string**
* `Permissions`: Stores the array of permissions found under [the permission list, `PermissionTypes`](https://github.com/Aptivi/Kernel-Simulator/blob/master/public/Kernel%20Simulator/Users/Permissions/PermissionTypes.cs).
  * The type of this variable is an **array** of **strings**

## Alias configuration

The kernel makes an alias configuration entry for each new alias created either by the `alias` command or by the call of the `AliasManager.ManageAlias()` function in the `Nitrocid.Shell.ShellBase.Aliases` namespace under the name of `Aliases.json` found in the kernel configuration directory. The format of the file looks like this:

```json
[
  {
    "Alias": "alias",
    "Command": "cmd",
    "Type": "ShellType"
  }
]
```

Let's explain each key one by one:

* `Alias`: Stores the alias
  * The type of this variable is a **string**
* `Command`: Stores the target command
  * The type of this variable is a **string**
* `Type`: The shell type
  * The type of this variable is a **boolean**

## Debug Device Configuration

The kernel makes a debug device configuration entry for each new device connected to the remote debugger under the name of `DebugDevicesNames.json` found in the kernel configuration directory. The format of the file looks like this:

```json
[
  {
    "Address": "192.168.1.100",
    "Name": "device",
    "Blocked": false,
    "MuteLogs": false,
    "ChatHistory": []
  }
]
```

Let's explain each key one by one:

* `Address`: Stores the device address
  * The type of this variable is a **string**
* `Name`: Stores the name of the remote debug device
  * The type of this variable is a **string**
* `Blocked`: Whether the device is banned or not
  * The type of this variable is a **boolean**
* `MuteLogs`: Whether to mute the kernel debug logs or not. Useful for conversations.
  * The type of this variable is a **boolean**
* `ChatHistory`: Recent chat history
  * The type of this variable is an **array** of **strings**.

## Event configuration

The kernel makes an event file for each event made in the `KSEvents` folder by `calendar event saveall`. The format is a JSON file that contains:

```json
{
    "EventDate": "2023-12-18T00:00:00",
    "EventTitle": "Test",
    "IsYearly": false,
    "StartMonth": 1,
    "StartDay": 1,
    "EndMonth": 1,
    "EndDay": 1,
    "Calendar": "Gregorian"
}
```

Let's explain each key one by one found under the root class, `EventInfo`:

* `EventDate`: Stores the date (and time) of the event
  * The type of this variable is a **date**
* `EventTitle`: The event title that's usually what's happening on the specified date.
  * The type of this variable is a **string**
* `IsYearly`: Whether or not this event is a yearly event
  * The type of this variable is a **boolean**
* `StartMonth`: For yearly events, specifies the start month.
  * The type of this variable is an **integer**
* `StartDay`: For yearly events, specifies the start day.
  * The type of this variable is an **integer**
* `EndMonth`: For yearly events, specifies the end month.
  * The type of this variable is an **integer**
* `EndDay`: For yearly events, specifies the end day.
  * The type of this variable is an **integer**
* `Calendar`: The calendar used while parsing the event date.
  * The type of this variable is a **string**

## Reminder configuration

The kernel makes a reminder file for each reminder made in the `KSReminders` folder by `calendar reminder saveall`. The format is a JSON file that contains:

```json
{
    "ReminderDate": "2023-12-17T16:00:00",
    "ReminderTitle": "W2",
    "ReminderImportance": 1
}
```

Let's explain each key one by one found under the root class, `ReminderInfo`:

* `ReminderDate`: Stores the date (and time) of the reminder
  * The type of this variable is a **date**
* `ReminderTitle`: The reminder title that's usually what you're reminded of.
  * The type of this variable is a **string**
* `ReminderImportance`: The reminder importance shows how important is the thing that you're reminded of.
  * The type of this variable is a **string** under the three values: `Low`, `Medium`, and `High`

## MAL and MOTD configuration

The configuration files for the message of the day before login (MOTD) and after login (MAL) are found under the kernel configuration directory. They are just plain-text files with the kernel placeholder support found in the below page linked to you below:

{% content-ref url="../inner-essentials/kernel-placeholders.md" %}
[kernel-placeholders.md](../inner-essentials/kernel-placeholders.md)
{% endcontent-ref %}

Both of these files are read every time the kernel log-in handler starts. The functions that are responsible for loading such info are both the `ReadMotd()` and the `ReadMal()` functions found under their respective namespaces `Nitrocid.Misc.Probers.Motd` and `Nitrocid.Misc.Probers.Mal`.

Changes to both files are done by the `SetMotd()` and the `SetMal()` functions under the same namespace. They're invoked either manually or by the usage of the `chmotd` and the `chmal` commands in the main shell.
