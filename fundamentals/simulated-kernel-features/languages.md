---
description: Multilingual Kernel!
icon: language
---

# Languages

<figure><img src="../../.gitbook/assets/066-welcomelang.png" alt=""><figcaption></figcaption></figure>

Localization was implemented when computers were distributed in non-English countries to aid the users in using their computers in their native language. This feature is currently supported in both Windows and Unix-based operating systems.

However, Linux boot messages don't get localized unless the localization is set, which is done in the middle of the boot process. This simulator attempts to localize the boot messages in the start of the process.

{% hint style="info" %}
Extra languages are bundled as a kernel extras addon.
{% endhint %}

## Setting the language

You can set the language using the `settings` command under the `General` section. Any language changes will be saved to the configuration file.

Languages usually get translated at the end of each development period of each upcoming kernel version, so it's normal to see untranslated strings in development versions. If you still see these untranslated strings in the final version, report them to us [using this page](https://github.com/Aptivi/Kernel-Simulator/issues/new).

{% hint style="info" %}
You can use `chlang` with the `-usesyslang` switch to change your kernel language to a language that Nitrocid detects based on your UI language set by your operating system.
{% endhint %}

### Through settings

<figure><img src="../../.gitbook/assets/067-setlang.png" alt=""><figcaption></figcaption></figure>

To change the simulated kernel language using the settings application, follow these steps:

1. Log-in to the system account, `root`, or any of the administrators or users that has at least the settings management permission
2. Execute the `settings` command, go to `General`, and go to `Language`
3. Select a new language, then log-out and log in again.

{% hint style="info" %}
Note that your account must have either the administrative permissions enabled or the settings management permission granted to be able to use this command.
{% endhint %}

### Through command-line

<figure><img src="../../.gitbook/assets/156-chlang.png" alt=""><figcaption></figcaption></figure>

To change the simulated kernel language using the command-line, follow these steps:

1. Log-in to the system account, `root`, or any of the administrators or users that has at least the settings management permission
2. Execute the `chlang` command, pointing it to the target language that you wish to set to, such as `dtc` and `ptg`.
3. Log-out and log in again.

{% hint style="info" %}
Note that your account must have either the administrative permissions enabled or the settings management permission granted to be able to use this command.
{% endhint %}
