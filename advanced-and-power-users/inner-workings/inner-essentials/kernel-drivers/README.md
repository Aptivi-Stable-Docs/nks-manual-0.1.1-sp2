---
description: How the kernel drivers work and their role in the kernel
icon: plug-circle-bolt
---

# Kernel Drivers

The kernel drivers allows your kernel to provide interfaces for different purposes, from console to filesystem. This is to present the kernel your implementation of different driver types. It's not a device driver, though!

The kernel drivers can be called using either the properties that point to the current driver of each driver type or the `GetDriver<TResult>()` function found in the `DriverHandler` class under the `KS.Drivers` namespace. These two will return an appropriate driver interface that actually allows you to call their functions that each driver of the same type implements. The types of kernel drivers are listed below:

<table><thead><tr><th width="271" data-type="content-ref">Driver</th><th width="255">Interface</th><th width="280">Base</th><th>Description</th></tr></thead><tbody><tr><td><a href="rng-drivers.md">rng-drivers.md</a></td><td><code>IRandomDriver</code></td><td><code>BaseRandomDriver</code></td><td>Random number generator drivers</td></tr><tr><td><a href="console-drivers.md">console-drivers.md</a></td><td><code>IConsoleDriver</code></td><td><code>BaseConsoleDriver</code></td><td>Console drivers</td></tr><tr><td><a href="network-drivers.md">network-drivers.md</a></td><td><code>INetworkDriver</code></td><td><code>BaseNetworkDriver</code></td><td>Network drivers</td></tr><tr><td><a href="filesystem-drivers.md">filesystem-drivers.md</a></td><td><code>IFilesystemDriver</code></td><td><code>BaseFilesystemDriver</code></td><td>Filesystem drivers</td></tr><tr><td><a href="encryption-drivers.md">encryption-drivers.md</a></td><td><code>IEncryptionDriver</code></td><td><code>BaseEncryptionDriver</code></td><td>Encryption drivers</td></tr><tr><td><a href="regular-expression-drivers.md">regular-expression-drivers.md</a></td><td><code>IRegexpDriver</code></td><td><code>BaseRegexpDriver</code></td><td>Regular expression drivers</td></tr><tr><td><a href="debug-logger-drivers.md">debug-logger-drivers.md</a></td><td><code>IDebugLoggerDriver</code></td><td><code>BaseDebugLoggerDriver</code></td><td>Debug logger drivers</td></tr><tr><td><a href="encoding-drivers.md">encoding-drivers.md</a></td><td><code>IEncodingDriver</code></td><td><code>BaseEncodingDriver</code></td><td>Encoding drivers</td></tr><tr><td><a href="hardware-prober-drivers.md">hardware-prober-drivers.md</a></td><td><code>IHardwareProberDriver</code></td><td><code>BaseHardwareProberDriver</code></td><td>Hardware probing drivers</td></tr><tr><td><a href="sorting-drivers.md">sorting-drivers.md</a></td><td><code>ISortingDriver</code></td><td><code>BaseSortingDriver</code></td><td>Integer sorting drivers</td></tr><tr><td><a href="input-drivers.md">input-drivers.md</a></td><td><code>IInputDriver</code></td><td><code>BaseInputDriver</code></td><td>Input drivers</td></tr></tbody></table>

### Mods and Drivers

{% hint style="warning" %}
Your kernel modifications **should** reference local versions of driver properties, like `CurrentConsoleDriverLocal`, unless it's not possible.
{% endhint %}

Each driver contains its own interface containing its own method definitions and their signatures. These interfaces are the ones that you must implement with your mod. The most basic interface for the kernel drivers is `IDriver` found in the same namespace, which is implemented by the driver-specific interfaces.

The final driver class implementation must implement both the base driver-specific base class and its interface, such as:

```csharp
internal class Terminal : BaseConsoleDriver, IConsoleDriver
```

{% hint style="info" %}
`IsBuiltin()` only returns true if the driver is found in the built-in driver list. `IsRegistered()`, however, returns true if the driver is either built-in or found in the custom drivers list.
{% endhint %}

If you have a kernel driver that you wish to register, you'll have to register the kernel driver, passing it the `IDriver` interface for your driver and the appropriate type. You can use the `RegisterDriver()` function to perform this operation, but you should set the current driver to your driver for the target wrappers, like the methods found in the `KS.Files` namespace that call the current filesystem driver, using the `SetDriverSafe<T>()` function.

{% hint style="warning" %}
The `SetDriver<T>()` function can be used, but doesn't prevent you from using internal drivers. We advice you to use the `SetDriverSafe<T>()` function instead.
{% endhint %}

{% hint style="danger" %}
Be sure to unregister your driver with the `UnregisterDriver()` function, or your driver will not get updated in the list of kernel drivers!
{% endhint %}

### Local drivers

If you want to temporarily to use a driver without affecting the entire kernel and its configuration, you can use this feature to temporarily switch drivers of supported types.

Assuming that your mod uses local drivers to do their actions, like `CurrentConsoleDriverLocal`, you must use the following functions **in order**:

* `BeginLocalDriverSafe<T>()`: This notifies the kernel that the mod is going to use the provided driver temporarily.
* `EndLocalDriver<T>()`: This notifies the kernel that the mod is done with the temporary driver.

Where `T` is the interface type of the driver found in the above table at the start of this page.

{% hint style="warning" %}
The `BeginLocalDriver<T>()` function can be used, but doesn't prevent you from using internal drivers. We advice you to use the `BeginLocalDriverSafe<T>()` function instead.
{% endhint %}

{% hint style="danger" %}
Be sure to execute the two above functions in the order specified above. Forgetting to call `EndLocalDriver<T>()` after you're done with the driver can cause the kernel to think that the mod is going to use your local driver forever.
{% endhint %}

### Fallback drivers

Additionally, the driver management API provides you an easy way to get the default driver that the kernel declares. You can use the below functions:

* `DriverHandler.GetFallbackDriver<TDriverInterfaceType>()`
  * This returns an instance of the default driver that the kernel initialized before drivers get set by the config reader.
* `DriverHandler.GetFallbackDriverName<TDriverInterfaceType>()`
  * This returns the actual driver name grabbed from the dictionary.

{% hint style="warning" %}
Don't confuse the name given by the `GetDriverName()` and its siblings with the name from the driver instance.
{% endhint %}

### Current drivers

If you have a driver interface type or the driver type from the `DriverTypes` enumeration, you can request for a current driver either from the available properties or from the `GetCurrentDriver()` and `GetCurrentDriverLocal()` functions.

```csharp
var currentDriver = DriverHandler.GetCurrentDriver(driverType);
```

* `GetCurrentDriver()` returns the current driver that's used by the kernel
* `GetCurrentDriverLocal()` returns the current local driver set by `BeginLocalDriver<T>()` or its safe sibling function.

{% hint style="info" %}
For all the driver management functions, you can also use the non-generic versions where they ask for a `DriverTypes` value in their first argument.
{% endhint %}

### Driver Configuration

<figure><img src="../../../../.gitbook/assets/118-inner.png" alt=""><figcaption></figcaption></figure>

The `settings -driver` application now lets you set your system-wide kernel drivers up in a completely new section called `Kernel driver settings`. It lets you configure current drivers for each supported driver type.

In order to configure your driver, select one of the driver types from the settings section shown above by pressing `ENTER`, and select your driver.

{% hint style="info" %}
You can also use `driverman` to change the kernel driver settings for scripting scenarios.

Like all other settings, you must save your kernel settings to apply your changes across reboots.
{% endhint %}
