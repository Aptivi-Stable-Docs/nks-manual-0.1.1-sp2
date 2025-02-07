---
description: What are the Kernel Modifications?
icon: toolbox
---

# Kernel Modifications

Kernel modifications let you extend the kernel functionality to your liking from custom commands to custom kernel applications. It also lets you unleash your console art by letting you make your own screensaver and your own kernel splash screen.

The kernel modifications also let you call the kernel functions and userspace functions, just like what device drivers and user applications in the major operating systems would do, respectively. However, the kernel modifications aren't zero-code solutions, so make sure that you have a bit of C# skills in order to be able to make kernel mods.

{% hint style="warning" %}
You can also use Visual Basic in your mod code, but we recommend that you avoid using it if possible as we no longer support that language.
{% endhint %}

{% hint style="info" %}
When building mods, you have to do the following:

* Make sure that your mod has a version that is SemVer v2.0 compliant. You can learn more about how to assign that version in the next few pages.
* Make sure that your mod is strongly signed using your own strong name key.
{% endhint %}

## Management

Deep explanation of the kernel modification management can be found in the below page:

{% content-ref url="kernel-modification-management/" %}
[kernel-modification-management](kernel-modification-management/)
{% endcontent-ref %}

## Analyzers

You can use the mod analyzers by consulting the page below:

{% content-ref url="analyzer-diagnostics/" %}
[analyzer-diagnostics](analyzer-diagnostics/)
{% endcontent-ref %}

## Building

If you'd like to build your own modifications, that's great! Click on the page below to get started.

{% content-ref url="your-mod.md" %}
[your-mod.md](your-mod.md)
{% endcontent-ref %}
