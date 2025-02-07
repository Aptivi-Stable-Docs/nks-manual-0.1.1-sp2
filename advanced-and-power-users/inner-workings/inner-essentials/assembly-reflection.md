---
description: Reflecting things...
icon: lock-keyhole
---

# Assembly Reflection

Nitrocid KS provides you with reflection tools to make using it easier for mods that want to use this feature for the following reasons:

* Deep manipulation with the arrays
* Assembly lookup management
* Field, property, and method management
* Integer manipulation
* Common reflection tools

This feature of Nitrocid KS is essential in cases where the default .NET implementation doesn't implement features that are not normally available for all the supported frameworks, such as array randomization that isn't available until .NET 8.0. This feature can also be used in future versions of Nitrocid KS to emulate features that have been implemented in future .NET versions and to do unusual tricks with various types, such as swapping two integers.

## Array manipulation

The reflection part of Nitrocid KS contains tools that allow you to manipulate with the arrays, such as sorting and randomizing array elements. Here are the following functions that you can use:

* `RandomizeArray()`: Randomizes the array using the [Schwartzian's transform](http://en.wikipedia.org/wiki/Schwartzian\_transform) formula.
* `RandomizeArraySystem()`: Randomizes the array using .NET 8.0's [Random.Shared.Shuffle()](https://learn.microsoft.com/en-us/dotnet/api/system.random.shuffle) function.
* `SortNumbers()`: Sorts the numbers using one of the sorting algorithms implemented by your array sorting drivers.

{% hint style="info" %}
Some features use the kernel driver tools, which you can learn more about it [here](kernel-drivers/sorting-drivers.md). `SortNumbers()` also allows you to use the sorting driver name as a shortcut to getting the sorting driver yourself.
{% endhint %}

## Assembly lookup management

Nitrocid KS contains the assembly lookup tools to manage how the kernel will load the assemblies specified in the search paths that `AssemblyLookup` manages. It allows you to add and to remove a path to/from the lookup path list using the following functions:

* `AddPathToAssemblySearchPath()`
* `RemovePathFromAssemblySearchPath()`

To load an assembly from the search paths, such as your mod dependencies, Nitrocid adds an event handler to the [`AppDomain.CurrentDomain.AssemblyResolve`](https://learn.microsoft.com/en-us/dotnet/api/system.appdomain.assemblyresolve) event to be able to call our assembly path resolver in an attempt to load any and all dependencies for your mods.

{% hint style="info" %}
In case Nitrocid attempts to load an old mod that depends on the old name of the application, `Kernel Simulator`, it'll give you an error saying that the mod is too old to be loadable. Therefore, mod loading will fail. If you're experiencing this in one of your mods and believe that this is the reason, contact the mod publisher for details about how to get the latest version.
{% endhint %}

## Integer tools

The `Reflection` part of the kernel contains a class, `IntegerTools`, that consists of useful integer tools from short numbers to 128-bit integer numbers to double-precision numbers, like converting literal file sizes in bytes to their human formats.

In addition to that, we've also placed useful extensions, such as converting the number of all kinds, short or long, to different number bases as strings, such as:

* `ToBinary()`
* `ToOctal()`
* `ToNumber()`
* `ToHex()`

### File size conversion

`SizeString()` extension functions can be used when importing the `Reflection` namespace. This allows you to easily convert the file sizes from bytes to their human-readable format.

You can use these functions, once the above namespace is imported, like this:

<pre class="language-csharp" data-title="MyModFuncs.cs" data-line-numbers><code class="lang-csharp">long bytes = 1024L * 1024 * 1024 * 1024;
string humanized = bytes.SizeString();
<strong>// Value of humanized: 1 TB
</strong></code></pre>

{% hint style="warning" %}
You can't use this function for enormous sizes, like 1 zettabytes, because of the limitation of the long integers. However, you shouldn't worry about this situation, since this is used generally for file sizes, and a single file in the world hasn't reached this size yet.
{% endhint %}

## Properties and Fields

The `Reflection` part of the Nitrocid API provides you with options to access public properties and fields that are declared in the public classes dynamically.

* `PropertyManager` manages properties, such as getting and setting property values.
* `FieldManager` manages fields, such as getting and setting field values.

In addition to the functions available in the above two classes, you can get all fields and properties defined in all the kernel types using the following functions:

* `GetAllFields()`
* `GetAllFieldsNoEvaluation()`
* `GetAllProperties()`
* `GetAllPropertiesNoEvaluation()`

## Method tools

In addition to property and field tools, the reflection feature of Nitrocid KS also allows you to manage methods and invoke them by their name. This set of tools allows you to do the following:

* `GetMethod()`: This function allows you to get a public method from either one of the Nitrocid kernel types or a specific type inside and outside Nitrocid.
* `InvokeMethod()`: This function allows you to invoke a non-static method, provided that you already have an object that is of a specific type that implements the public method.
* `InvokeMethodStatic()`: This function allows you to invoke a public static method.

## Common reflection tools

Finally, the common reflection tools are here to help you get the most out of the reflection feature of .NET by providing you the following functions:

* `IsDotnetAssemblyFile()`: This function checks to see if a path to a specific file is a .NET assembly or not. If it is a .NET assembly, it returns true with an `AssemblyName` instance containing information about your desired assembly.
