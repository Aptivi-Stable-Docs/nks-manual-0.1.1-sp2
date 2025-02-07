---
description: Your homepage, your way!
icon: house
---

# The Nitrocid Homepage

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Nitrocid KS 0.1.1 introduces you with an awesome homepage that gets started every time you log in, though you can disable it through the kernel settings. It provides you with the following elements:

* Extensible application list
* Settings and About buttons
* An analog clock widget
* Top three RSS feeds
* Keybindings that allow you to control the homepage

{% hint style="info" %}
In order to show the top three RSS feeds, an RSS kernel addon must be installed, the "Headlines on login" configuration must be enabled, and the kernel must be connected to the Internet.
{% endhint %}

## Keybindings

You can navigate the application list using either the up/down arrow keys, page up/page down arrow keys, or home/end keys. You can also use your mouse to navigate the homepage, but you must enable the mouse support in order to be able to select an item using just your mouse.

The keybindings shown at the botton of the screen allow you to perform various operations, such as exiting to the shell, logging out, and executing a selected action.

## Custom applications

Your mods can extend the application list so that your homepage becomes more useful. You can use the functions that are made available in the `HomepageTools` class under the `Nitrocid.Shell.Homepage` namespace. You can easily register your action by passing in both the application name and your function delegate to the `RegisterAction()` function. This is an example that shows you how to use this function in your mod code, assuming that you have a function that opens a game called Rush:

{% code title="RushModEntry.cs" lineNumbers="true" %}
```csharp
HomepageTools.RegisterAction("Rush", RushGame.Start);
```
{% endcode %}

{% hint style="warning" %}
You must check to see if the action is registered or not using the `IsHomepageActionRegistered()` function, passing it the same name that you want to register. Attempting to re-register the action causes the exception to be thrown with the type of `KernelExceptionType.Homepage`.
{% endhint %}

To unregister the action, you can use the `UnregisterAction()` function, supplying it the name of your custom action that you want to remove. This is an example that shows you how to use this function in your mod code:

{% code title="RushModEntry.cs" lineNumbers="true" %}
```csharp
HomepageTools.UnregisterAction("Rush")
```
{% endcode %}

{% hint style="warning" %}
You must check to see if the action is registered or not using the `IsHomepageActionRegistered()` function, passing it the same name that you want to unregister. Attempting to unregister the action that doesn't exist causes the exception to be thrown with the type of `KernelExceptionType.Homepage`.
{% endhint %}
