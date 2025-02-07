---
description: Changing how the console input works
icon: plug-circle-bolt
---

# Input Drivers

The console input driver is one of the supported driver types on Nitrocid KS. These drivers allow you to change how the console input works, thus earning dynamic console input improvements, such as providing better methods to read the console input (like Spectre.Console).

The console input drivers have the following characteristics:

* Interface: `IInputDriver`
* Base class: `BaseInputDriver`

The console input drivers have the following functions that you can optionally override below:

{% code title="IInputDriver.cs" lineNumbers="true" %}
```csharp
string ReadLine();
string ReadLine(string InputText);
string ReadLine(string InputText, string DefaultValue);
string ReadLine(string InputText, string DefaultValue, TermReaderSettings settings);
string ReadLineWrapped();
string ReadLineWrapped(string InputText);
string ReadLineWrapped(string InputText, string DefaultValue);
string ReadLineWrapped(string InputText, string DefaultValue, TermReaderSettings settings);
string ReadLineUnsafe(string InputText, string DefaultValue, bool OneLineWrap = false);
string ReadLineUnsafe(string InputText, string DefaultValue, bool OneLineWrap = false, TermReaderSettings settings = null);
string ReadLineNoInput();
string ReadLineNoInput(TermReaderSettings settings);
string ReadLineNoInput(char MaskChar);
string ReadLineNoInput(char MaskChar, TermReaderSettings settings);
string ReadLineNoInputUnsafe();
string ReadLineNoInputUnsafe(TermReaderSettings settings);
string ReadLineNoInputUnsafe(char MaskChar);
string ReadLineNoInputUnsafe(char MaskChar, TermReaderSettings settings);
ConsoleKeyInfo ReadKeyTimeout(bool Intercept, TimeSpan Timeout);
ConsoleKeyInfo ReadKeyTimeoutUnsafe(bool Intercept, TimeSpan Timeout);
ConsoleKeyInfo DetectKeypress();
ConsoleKeyInfo DetectKeypressUnsafe();
```
{% endcode %}

The `InputDriverTools` class contains tools to get all the console input drivers and their names and set a console input driver as a default. The driver management tools also allow you to do the same thing, though you'll have to specify the driver type.
