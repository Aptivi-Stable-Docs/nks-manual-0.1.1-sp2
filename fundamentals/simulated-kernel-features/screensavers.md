---
description: Screensavers and their usage
icon: display
---

# Screensavers

<figure><img src="../../.gitbook/assets/068-saver.png" alt=""><figcaption></figcaption></figure>

The screensavers were touted to be a solution against screen burn-ins in cathode ray-tube (CRT) or plasma displays. They fill the screens with either the blank screen or moving image or parts across the entire screen. They also are placed as a security measure so that when screensavers exit, the user will be required to input the password to be able to use your computer again.

The first screensaver that blanked the screen after three minutes of inactivity on the original IBM PCs, `scrnsave`, was created on 1983 by John Socha. Since then, improvements were made to make modern screensavers than just blanking the screen, to the point that the screensavers earned 3D support in modern times.

The simulated kernel attempts to simulate this functionality in its complete state. You can even customize most built-in screensavers using the built-in `settings` application found in the kernel, though you have to pass the `-saver` switch to it.

{% hint style="danger" %}
Some of the screensavers bundled with the addons, such as ExcaliBeats and KSX 2, and some of your custom screensavers may contain fast-paced animations and flashing colors, which may cause seizures for the photosensitive.

Before attempting to set your kernel screensaver default to such screensaver, if you are allergic to such animations, you must seek professional medical specialists found in your local region for guidance.

If you don't want to be warned of this everytime you run such screensaver, you can turn this warning off by opening the kernel settings and turning on the `Acknowledge the photosensitive seizure warning` option from the `Screensaver settings` section.

**Please note that the random screensaver will not emit this warning, despite having this option turned off.**
{% endhint %}

### Setting the screensaver

<figure><img src="../../.gitbook/assets/069-setsaver.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Extra screensavers are bundled as a screensaver pack addon.
{% endhint %}

To set the screensaver to your favorite screensaver, use the `setsaver` command. Since this command is an administrative command, you either need to use an administrator account or use an account that has been granted the administrative command permissions.

1. Log-in to the system account, root, or any of the administrators or users that has at least the strict command running permissions
2. Execute the `setsaver` command to set the default kernel screensaver
   * The full usage of the `setsaver` command is `setsaver <(CustomSaverName)/saver>`
3. Lock or save your screen using `savescreen` or `lockscreen`.

{% hint style="info" %}
Note that your account must have either the administrative permissions enabled or the strict command running permission granted to be able to use this command.
{% endhint %}

### Saving your screen

To save your screen using your default screensaver or any other screensaver, you need to use the `savescreen` command to launch the screensaver.

### Locking your screen

To lock your screen in the simulated kernel, you need to use the `lockscreen` command to launch the screensaver. Once you press any key, you need to enter your user password before you're able to access the shell again.

{% hint style="info" %}
As of 0.1.0 Beta 3, you can enable or disable automatic screen locking by going to the kernel settings > Screensaver and enabling or disabling the `Enable Screensaver Timeout` setting.

If you have this option enabled, you can set the timeout in the format of `DDD.HH:MM:SS.NNN`, or `HH:MM:SS` in the simplest form.
{% endhint %}

### Reloading your custom screensaver

To reload your custom screensaver, you need to reload the mod that registered your screensaver.

1. Execute the `modman reload <modName>` command to re-register your custom screensaver (if the mod registers that screensaver upon starting up)
2. Save your screen using `savescreen [CustomSaver]`.

{% hint style="info" %}
Note that your account must have either the administrative permissions enabled or the strict command running permission granted to be able to use this command.
{% endhint %}
