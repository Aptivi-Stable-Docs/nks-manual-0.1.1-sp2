---
description: This page describes how to manage your kernel mods
icon: wrench
---

# Managing your Mod

Kernel modification files are stored in the following directories that are found under the kernel configuration directory:

* `KSMods`: Stores all the modifications under the `.dll` extension

Important mods, including those that load splashes on boot, get loaded after the configuration files load, so people who use custom splashes in their kernel configuration will actually see their custom splashes when the kernel starts up.

However, for optional mods, they get loaded late so they can load properly. This loading is done by scanning the `KSMods` directory and querying every `.DLL` file with `ParseMod()`.

## Mod parsing

The mod finalization phase gets executed as soon as the mod parser sees the file as a mod (a `.dll` assembly that implements `IMod`), with a call to the `FinalizeMods()` function. Here's what it does:

1. If it sees the script as an instance of `IMod`, it fires the `ModParsed` event
2. Adds mod dependency path to the assembly lookup path (`KSMods/Deps/Mod-FileVersion/`)
3. Checks the expected mod API minimum version with the kernel API version to see if there is a mismatch
   * If the checker found that the mod needs a higher API version, the mod parsing fails with the appropriate message
   * If the checker couldn't determine the minimum API version required by the kernel mod, it goes ahead, but with a warning that the mod may fail to start.
4. Checks the mod localization file from the path: `KSMods/Localization/Mod-FileVersion/`
5. If the mod has no name, **mod parsing fails.**
6. Checks the mod for the version and its SemVer 2.0 compliance. **If the version is not a SemVer 2.0 compliant, or if the version is empty, mod parsing fails.**
7. Satisfies the mod dependencies by loading them as appropriate.
8. Calls the `script.StartMod()` function in your script
9. Adds the mod to the mod manager

{% hint style="info" %}
The mod system will automatically unload the target mod's directory for assembly lookup. If, for some reason, this fails, you can manually unload their paths from the lookup using the below function:

```csharp
RemovePathFromAssemblySearchPath(Path);
```
{% endhint %}

### Mod-to-Mod Dependencies

If you want mods to depend on other mods, you can create a JSON file, called the dependency list file, that holds information about the mod name and the required mod version. The format for the dependency list file should be:

{% code title="Mod-moddeps.json" lineNumbers="true" %}
```json
[
    {
        "name": "Mod2",
        "version": "1.0.0"
    }
]
```
{% endcode %}

The dependencies list file should be saved as the name which satisfies this format: `ModName-moddeps.json`. For example, if your mod, called `ProjectVision`, depends on another mod, called `NitroBoost`, you must create that file called `ProjectVision-moddeps.json` that contains the following contents:

{% code title="ProjectVision-moddeps.json" lineNumbers="true" %}
```json
[
    {
        "name": "NitroBoost",
        "version": "1.2.4"
    }
]
```
{% endcode %}

{% hint style="danger" %}
If one of the mod dependencies failed to load, the mod parser will report a failure for that mod.
{% endhint %}

### Mod load priorities

You can also control when your mod loads (early or late) by overriding the `AddonType` enumeration to point to one of the correct mod priorities, depending on your mod:

* `Important`
* `Optional`

Important mods get loaded before the kernel configuration loads, while the optional ones get loaded after the hardware gets parsed before the system verification stage.

{% code title="Your mod code" lineNumbers="true" %}
```csharp
override ModLoadPriority LoadPriority =>
    ModLoadPriority.Optional
```
{% endcode %}

## Mod Inter-communication

In addition to the above mod management tools, Nitrocid KS provides you with tools that allow you to communicate with either the other mods or the other addons that implement their publicly-available functions, fields, or properties as in the following overridable mod properties:

{% code title="IMod.cs" lineNumbers="true" %}
```csharp
ReadOnlyDictionary<string, Delegate> PubliclyAvailableFunctions { get; }
ReadOnlyDictionary<string, PropertyInfo> PubliclyAvailableProperties { get; }
ReadOnlyDictionary<string, FieldInfo> PubliclyAvailableFields { get; }
```
{% endcode %}

To get started, follow the two pages below, depending on the type of the communication, to get started:

{% content-ref url="inter-mod-communication.md" %}
[inter-mod-communication.md](inter-mod-communication.md)
{% endcontent-ref %}

{% content-ref url="inter-addon-communication.md" %}
[inter-addon-communication.md](inter-addon-communication.md)
{% endcontent-ref %}
