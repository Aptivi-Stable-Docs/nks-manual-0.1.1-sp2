---
description: How can we modify the behavior of a command?
icon: toggle-off
---

# Command Switch Information

For `SwitchInfo` instances, consult the below constructors to create an array of `SwitchInfo` instances when defining your commands:

```csharp
public SwitchInfo(string Switch, string HelpDefinition)
public SwitchInfo(string Switch, string HelpDefinition, SwitchOptions options)
public SwitchInfo(string Switch, string HelpDefinition, bool IsRequired = false, bool ArgumentsRequired = false, string[] conflictsWith = null, int optionalizeLastRequiredArguments = 0)
```

{% hint style="info" %}
You can access the switch options using the `Options` property. The existing properties, like `IsRequired`, are proxies to that property, but are to stay for compatibility.

Try to use the second overload if you want to specify the options, if possible. This allows you to be more expressive in your mod command definition code, making it more readable.
{% endhint %}
