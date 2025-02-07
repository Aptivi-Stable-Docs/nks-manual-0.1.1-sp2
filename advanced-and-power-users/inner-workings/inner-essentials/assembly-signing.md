---
description: Sign here, please.
icon: pen-fancy
---

# Assembly Signing

In .NET Framework, there is a function that gives your assembly a strong signature called [Strong Naming](https://learn.microsoft.com/en-us/dotnet/standard/library-guidance/strong-naming). It allows you to sign your assembly — in this case **your mod** — to give it a strong name and be trustworthy according to the Nitrocid mod parser.

{% hint style="info" %}
While the modern .NET does not use strict assembly loading if you have strongly named your assembly, Nitrocid KS, in its default setting, disallows loading unsigned mods. It's recommended to strongly name your mods with your strong naming key. You can generate one using [SN.exe](https://learn.microsoft.com/en-us/dotnet/framework/tools/sn-exe-strong-name-tool).
{% endhint %}

In the `Signing` part of the `Security` namespace, there is an assembly signing tools class, called `AssemblySigning`, that allows you access to various tools that are used for assembly signing, including checking to see if a given assembly is signed.

This class gives you these tools:

* `PublicKeyToken()`: It takes either a loaded assembly, an assembly name instance, or a path to a valid assembly to get a public key token.
* `IsStronglySigned()`: This takes the three above items and returns true if a given assembly is signed.
