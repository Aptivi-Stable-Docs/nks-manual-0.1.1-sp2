---
description: >-
  Other inner workings that don't fit with the inner essentials and might be
  useful for your mods
icon: fan
---

# Miscellaneous APIs

In addition to the essential APIs that are provided with the Nitrocid kernel that allow you to make awesome mods ranging from simple ones to the most complicated ones, Nitrocid provides some of the useful APIs that don't fit with the category but can be helpful when developing mods.

The following APIs can be used in your mods:

## Kernel version information

You can get the kernel version information using the following properties from the `KernelMain` class:

* `Version`: Gets the kernel version without the build specifiers
* `VersionFull`: Gets the kernel version with the build specifiers (essentially the same as the GitHub tag for the release)
* `ApiVersion`: Gets the kernel API version.

## Writing to the console

The `Writers` namespace of the Nitrocid-specific `ConsoleBase` namespace contains the following three writer types:

* `TextWriters`: Provides you with all the normal console writers with kernel color types.
* `TextFancyWriters`: Provides you with all the fancy writers with kernel color types.
* `TextMiscWriters`: Provides you with all the miscellaneous writers with kernel color types.
* `TextDynamicWriters`: Provides you with all the dynamically animated console writers with kernel color types.

If you want to either plainly write text or with a custom color of your choice, consult the Terminaux guide for more information:

{% content-ref url="https://app.gitbook.com/s/G0KrE9Uk2AiblqjWtpAo/usage/console-tools/console-writers" %}
[Console Writers](https://app.gitbook.com/s/G0KrE9Uk2AiblqjWtpAo/usage/console-tools/console-writers)
{% endcontent-ref %}
