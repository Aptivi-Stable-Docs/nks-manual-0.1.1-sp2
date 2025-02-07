---
description: Deep explanation about the kernel environments.
icon: envira
---

# Kernel Environments

Kernel environments represent classes that are responsible for storing the entry point delegate of the environment, which runs all the necessary features and handles the entire system. They also contain facilities that modify the behavior of the environments by passing arguments to it.

Kernel environments are managed in the `EnvironmentTools` class, and your mod can access it to create your own kernel environment for your purposes. This is useful in certain situations, especially when you want to make a separate interface for kids that contains games and kid-friendly applications.

The base environment is modeled in a way that you need to override the following functions to create your own environment:

* `Name`: The name of your environment that the bootloader shows.
* `EnvironmentEntry`: A delegate to the entry point of your environment.

{% hint style="info" %}
The kernel environment system basically assumes that your entry point code contains a loop that checks to see if neither a reboot nor a shutdown is requested. The base entry point code looks like this (only the relevant part):

```csharp
private static void MainLoop()
{
    while (!PowerManager.RebootRequested && !PowerManager.KernelShutdown)
    {
        (...)
    }
}
```
{% endhint %}

After that, you'll need to create a new instance of your environment and set it as the default environment using `SetEnvironment()`. Optionally, you can set the arguments using the `SetEnvironmentArgs()` function. This change of the environment is temporary on the next boot and will be reset to the main kernel environment the next time the kernel reboots.

This is, however, not enough to add your kernel environment to the bootloader screen, though. You'll need to add your kernel environment to the mod initialization code using `BootManager`'s `AddBootApp()` function in order to be able to use your kernel environment each time the mod that provides it is loaded. After that, if everything goes well, you should be able to see your kernel environment listed in the kernel environment selection screen once all the necessary configuration for the kernel is loaded.

{% hint style="warning" %}
You'll need to remove your environment from the list of bootable apps using `RemoveBootApp()` when unloading your mod.
{% endhint %}

Once done, you can now add different kernel environments for different needs, such as your mod that implements the "Kids Mode" environment.

{% hint style="info" %}
If you don't have any other environment loaded, Nitrocid starts without any bootloader prompt.
{% endhint %}
