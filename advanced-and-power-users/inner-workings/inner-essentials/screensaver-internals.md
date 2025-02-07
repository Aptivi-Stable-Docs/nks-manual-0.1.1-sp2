---
description: Your mods and your screensavers
icon: desktop
---

# Screensaver Internals

<figure><img src="../../../.gitbook/assets/068-saver.png" alt=""><figcaption></figcaption></figure>

Since Beta 3, your mods can now control whether to register or to unregister your custom screensaver or not. However, let's explain things one by one to learn more about how your mods can interact with screensavers.

## Kernel Mods and their role

Kernel mods can register their screensaver if they have one or more screensaver classes and have created a new instance of such classes using a function that we'll highlight later. However, before registering and/or unregistering, you may want to check the screensaver for existence.

Screensaver management functions are available on the `ScreensaverManager` class.

### Checking for Existence

```csharp
public static bool IsScreensaverRegistered(string name)
```

This function checks the screensaver name for the following conditions:

* If Nitrocid KS has defined a built-in screensaver by its lowercase name, like if you're checking for "`bloom`," then it returns `true`.
* If Nitrocid KS couldn't find a built-in screensaver by its lowercase name, but could find said screensaver in the custom screensaver list, then it returns `true`.
* If neither list indicate that this screensaver exists, but the requested name was "`random`" with respect to `ShowSavers()` treating it specially, then it returns `true`.
* Otherwise, `false`.

### Registering screensavers

```csharp
public static void RegisterCustomScreensaver(string name, BaseScreensaver screensaver)
```

This function checks to see if the requested screensaver is already registered using the checks mentioned above. If the screensaver is not registered, it will add the screensaver entry containing the name and the base screensaver instance to the list of screensavers.

{% hint style="info" %}
Trying to register a custom screensaver if said screensaver already exists causes an exception to be thrown.
{% endhint %}

### Unregistering screensavers

```csharp
public static void UnregisterCustomScreensaver(string name)
```

This function checks to see if the requested screensaver is is already registered using the checks mentioned above. If the screensaver is registered, it will remove the requested screensaver from the custom screensaver list.

{% hint style="info" %}
Trying to unregister a screensaver that doesn't exist causes an exception to be thrown.
{% endhint %}

## Screensaver Management

The screensaver feature of the kernel features two classes: `ScreensaverManager` and `ScreensaverDisplayer`. The former class, which is considered high-level, handles screensaver management and display, while the latter class handles screensaver displaying to save the screen or to lock the screen using low-level functions, which are only available internally.

`ScreensaverManager` can be found on the API reference documentation, which is found in the bottom of the left pane. Here are the top three features:

### Saving your screen

The screensaver management class contains a function with two overloads dedicated to saving your screen:

{% code title="ScreensaverManager.cs" lineNumbers="true" %}
```csharp
public static void ShowSavers()
public static void ShowSavers(string saver)
```
{% endcode %}

When this function is called, it first checks to see if the screensaver exists, according to the conditions highlighted in the beginning of the page.

If the provided screensaver is `random`, the screensaver manager will select a random screensaver from the list and show it. Otherwise, it'll check both the built-in screensaver list and the custom screensaver list.

{% hint style="info" %}
This is also triggered by the screensaver timeout handler, which is started when the kernel boots. This handler waits for a specified number of milliseconds until the limit is reached. If there is no interaction with the console, the screensaver will activate.
{% endhint %}

### Locking your screen

{% code title="ScreensaverManager.cs" lineNumbers="true" %}
```csharp
public static void LockScreen()
```
{% endcode %}

If you want your mod to lock your screen, call the above function to initiate locking. This function calls `ShowSavers()`, which saves your screen.

Once you press any key, this function checks to see if you have password requirement after locking enabled. If it's enabled, the login handler will check to see if your password is empty. If it's not empty, it'll prompt for your password before unlocking.

#### Lock prevention

Your mod can prevent screen locking by calling the below function:

{% code title="ScreensaverManager.cs" lineNumbers="true" %}
```csharp
public static void PreventLock()
```
{% endcode %}

Once the lock prevention is enabled, the kernel screensaver timeout thread will stop and no screen locking attempt will be made when the timeout reaches. To revert back to the original behavior, you can call the below function:

{% code title="ScreensaverManager.cs" lineNumbers="true" %}
```csharp
public static void AllowLock()
```
{% endcode %}

### Setting default screensaver

{% code title="ScreensaverManager.cs" lineNumbers="true" %}
```csharp
public static void SetDefaultScreensaver(string saver)
```
{% endcode %}

This function allows your mod to set a default screensaver. Once done, it saves the new screensaver name to the appropriate key, `DefaultSaverName`, in the main configuration instance. As soon as it's saved, the configuration is saved.

## Display Control

{% hint style="info" %}
This doesn't apply to interactive applications that use the Screen feature as the renderer.
{% endhint %}

Interactive applications might need to listen to the re-render requests so that they can check to see if they need a re-draw. In order to force re-rendering, you can check for the `ScreenRefreshRequired` property in the `ScreensaverManager` class.

For instance, applications that use Nitrocid's Interactive TUI usually respect this request by listening for the refresh request here:

{% code title="InteractiveTuiTools.cs" lineNumbers="true" %}
```csharp
if (ScreensaverManager.ScreenRefreshRequired || BaseInteractiveTui.RedrawRequired)
{
    _refreshSelection = true;
    DebugWriter.WriteDebug(DebugLevel.I, "We're redrawing.");
    KernelColorTools.LoadBack(BaseInteractiveTui.BackgroundColor);
    (...)
}
```
{% endcode %}

{% hint style="warning" %}
This property returns `true` if the screensaver has been entered. However, if you need to get the value of this property, you must do so once, because it gets reset to `false` once it's called.
{% endhint %}
