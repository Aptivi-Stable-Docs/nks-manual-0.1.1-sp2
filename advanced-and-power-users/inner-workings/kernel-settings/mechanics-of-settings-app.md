---
description: How the settings application works?
icon: pickaxe
---

# Mechanics of Settings App

The Settings application displays all the available sections on the first page and the available configuration options inside it on the second page. But, how?

The Settings application uses the embedded JSON file inside the kernel to provide it the available configuration options inside each section. Reading these files are done by the Settings app upon starting it up using the `settings` command, which:

| Settings type        | Switch               |
| -------------------- | -------------------- |
| Normal               |                      |
| Screensaver          | `-saver`             |
| Splash               | `-splash`            |
| Extra screensavers   | `-addonsaver`        |
| Other settings types | `-type=settingstype` |

The format of all the settings can be found in the below link.

{% content-ref url="settings-format.md" %}
[settings-format.md](settings-format.md)
{% endcontent-ref %}

Here's how the settings application works:

1. When the settings application is loaded, it loads one of these files to the cached settings entry object to be used. The application then tries to list all the sections passed to it.
2. If the user enters a section, the app will make a list of each available settings entry found in the `Keys` variable with their values, queried by the `ConfigTools.GetValueFromEntry()` function in the `Nitrocid.Kernel.Configuration` namespace.
3. When the user changes a value in the selected entry, the settings application determines what to do based on the type and gives the user an option to input the new value. The next page actually explains what to do based on the type.
4. When the user sets a new value, the app attempts to set a value by delegating the `PropertyManager.SetPropertyValue()` function in the `Nitrocid.Misc.Reflection` namespace.

When the user is done configuring the kernel to their needs, they'll save the kernel configuration. The app invokes the `Config.CreateConfig()` function to save the changes to the configuration file.

<figure><img src="../../../.gitbook/assets/101-settings.png" alt=""><figcaption></figcaption></figure>

The CLI version of the settings, which hosts the four commands as explained in the Kernel Settings page, uses a bunch of settings tools, including `GetSettingsKey()` that allows you to quickly get a settings key containing settings key information for a specified variable. This key is usable for useful functions, like setting a variable to a specified value that `setconfigvalue` is doing.
