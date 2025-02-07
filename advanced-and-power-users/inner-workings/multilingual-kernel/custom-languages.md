---
description: How to define your custom languages.
icon: globe
---

# Custom Languages

When you extract the binary folder of Nitrocid KS, you may see a folder called `CustomLanguages`. Inside it, you can see the `Metadata.json` file that holds language metadata as described in the previous article. Before reading this, you must understand the multilingual kernel feature and how it works by clicking on the below link.

{% content-ref url="./" %}
[.](./)
{% endcontent-ref %}

Customized languages play an important role in being a building block for making your custom localization. This feature contains two separate folders:

* `<KSExecutablePath>/CustomLanguages`
  * This is the folder which you should place the source `.txt` files containing all your string translations. You should implement all the translations found in the `eng.txt` file, usually shipped with the Nitrocid KS binary.
* `<KSConfigPath>/KSLanguages`
  * This is the folder that hosts all your custom language JSON files containing the translation metadata and the necessary translation data.

When the kernel starts up, it attempts to install all the custom languages found in the `KSLanguages` path to the language store found within the language manager. This way, the kernel configuration reader will understand that your custom language exists.

## Build your language

Cool! So, you want to build your language? First, open the `Metadata.json`︎ file found in the `CustomLanguages` folder. Write details about your language in their appropriate places. For quick reference, here's an excerpt from the metadata JSON prototype:

```json
]
    {
        "three": "lng",
        "name": "My Language",
        "transliterable": false,
        "codepage": 932,
        "culture": "en-GB"
    }
[
```

Again, the `codepage` parameter and the `culture` parameter is optional. If you want your language to be able to be transliterated, make two metadata information about your target language, but append the `-T` postfix to one of the metadata information name, for example:

```json
]
    {
        "three": "lng",
        "name": "My Language",
        "transliterable": true,
        "culture": "en-GB"
    },
    {
        "three": "lng-T",
        "name": "My Language",
        "transliterable": true,
        "codepage": 932,
        "culture": "en-GB"
    }
[
```

After you filled in the metadata information about languages you want to create, make a text file with the name of the short language name (`lng` and not `My Language`) for each language with the `.txt` extension, like this: `lng.txt`

Now, head to `Translations/eng.txt` and use it as a reference to translate all the strings found there to your target language. Put all your translations to the newly-created file.

{% hint style="danger" %}
Both `eng.txt` and your language text file **MUST** have the exact same line numbers!
{% endhint %}

After this is done, launch KSJsonifyLocales either by double-clicking the executable file (usually `Nitrocid.LocaleGen.exe`) or by opening the terminal to the executable path and executing the `dotnet Nitrocid.LocaleGen.dll` command.

{% hint style="info" %}
You can also let this program generate only the custom locale information by passing the `--CustomOnly` argument to it.
{% endhint %}

## Test your language

After `Nitrocid.LocaleGen` finishes compiling the JSON file containing your translation, you should be able to find the resulting file in the `<KSConfigPath>/KSLanguages` folder.

Open Nitrocid KS, and change your system language or your user language to your customized language.

### Changing your kernel language

If you want your language to be used in the entire kernel, follow these steps:

1. Log in to any user account _that has administrative permissions or access to the kernel settings program_
2. Execute the `settings` command
3. Go to `General` section
4. Go to `Language` entry
5. Select your language. Your language should be there among the list of the kernel languages.

### Changing your user language

If you want your language to be used just for your user account, follow these steps:

* Log in to any user account _that has administrative permissions or access to the kernel settings program_
* Execute the `admin` command
* Execute the `userlang` command, pointing it to a user account and a language as needed
* Log out and log in as that user

## Other options for the generator

This locale generator comes with options to change how it works. These switches provide the following functions:

* `--CustomOnly`
  * Restricts the generator to custom languages
* `--Singular <lang>`
  * Restricts the generator to a single custom or normal language

## Installing custom languages by hand

If you have a custom language file and you want to quickly install it without having to restart the entire kernel, you can use the `langman` command to use the following parameters:

* `load <lang>`
  * Loads the custom language.
* `unload <lang>`
  * Unloads the custom language.
* `reload <lang>`
  * Reloads the custom language. Synonymous to the sequence of the `unload` and the `reload` parameters.

To check that your language is installed, use `list <lang>` to verify that your language is there and that its information is available.

{% hint style="info" %}
For mods that want to install or uninstall custom languages, you can use the API functions found within the `LanguageManager` module.
{% endhint %}

### Your Mods and Your Strings

Your mods can now be translated! Nitrocid KS can now detect your language files from your mods. The mod parser within the kernel looks for the localization files found under the `KSMods/Localization/Mod-FileVersion/` path. The format is in JSON, and the file name is the same as the three-letter language name. The format is the same as the regular Nitrocid KS localization files embedded as resources. For example, for the French language, the format would look like below:

{% code title="fre.json" lineNumbers="true" %}
```json
{
  "Name": "French",
  "Transliterable": false,
  "Localizations": {
    "Invalid color template {0}": "Modèle de couleur non valide {0}",
    (...)
  }
}
```
{% endcode %}

{% hint style="info" %}
You must use the `mklang` command within Nitrocid, pointing to a directory that contains your language metadata and your `eng.txt` file containing all strings from your mod in order to be able to translate them within the app. When you're done, save your changes by choosing the `Save` option.
{% endhint %}

In the Localizations property, it holds a group of keys and values. The key is an original string extracted from your mod (`"Invalid color template {0}"`) and the value is the translated string (`"Modèle de couleur non valide {0}"`).

When Nitrocid KS detects your localization file, it tries to parse it and adds your translated strings to the `ModInfo` class, which is used by the `Translate` static class to translate your mod strings.
