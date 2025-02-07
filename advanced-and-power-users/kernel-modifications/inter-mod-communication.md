---
description: Two or more than two mods talking to each other in Nitrocid
---

# 📞 Inter-Mod Communication

Inter-Mod Communication allows your mods to execute the publicly-available functions of another mod. It allows you to connect two or more than two mods with each other in a mechanism that doesn't interfere with the other mod's operations.

### How to make a function publicly accessible?

You can make a function publicly accessible in one mod by implementing the `PubliclyAvailableFunctions` with a read-only dictionary that contains all your publicly-available functions in your main mod entry point.

{% code title="Interface declaration" %}
```csharp
ReadOnlyDictionary<string, Delegate> PubliclyAvailableFunctions { get; }
```
{% endcode %}

The key is the function name that the mod manager uses to search for a function when invoking the function executor. The value, which is a delegate of either a function or a method (subs in Visual Basic), can either take no parameters or take parameters of varying sizes.

To execute custom mod functions in another mod, you must specify the mod name and the function to execute in the `ExecuteCustomModFunction()` method:

```csharp
public static object ExecuteCustomModFunction(string modName, string functionName)
public static object ExecuteCustomModFunction(string modName, string functionName, params object[] parameters)
```

{% hint style="info" %}
You must specify the main mod name in the above function, since it uses that name to fetch all mod parts and query them for available functions.

`ExecuteCustomModFunction()` returns `null` under the following conditions:

* There are no functions in all the mod parts from your mod.
* There is no function that goes by the name of the specified function name that you plan to execute.
* There is a function, but the delegate is unspecified.
{% endhint %}

### How can I make a property or a field publicly accessible?

In your mod, you can make a property or a field publicly accessible by implementing the `PubliclyAvailableProperties` property with a read-only dictionary that contains all your publicly-available properties in your main mod entry point. Similarly, you can add a field to its own dedicated dictionary, `PubliclyAvailableFields`.

{% code title="Interface declaration" %}
```csharp
ReadOnlyDictionary<string, PropertyInfo> PubliclyAvailableProperties { get; }
ReadOnlyDictionary<string, FieldInfo> PubliclyAvailableFields { get; }
```
{% endcode %}

The key is the property or the field name that the mod manager uses to search for a property or a field to get or set its value.

To get a property value or a field value from a mod, you can call the following functions:

```csharp
public static object GetCustomModPropertyValue(string modName, string propertyName)
public static object GetCustomModFieldValue(string modName, string fieldName)
```

Similarly, to set a property value or a field value declared publicly by a mod, you can call the following functions:

```csharp
public static void SetCustomModPropertyValue(string modName, string propertyName, object value)
public static void SetCustomModFieldValue(string modName, string fieldName, object value)
```

{% hint style="info" %}
You must specify the main mod name in the above functions, since they use that name to fetch all mod parts and query them for available fields or properties.

`GetCustomModPropertyValue()` and `GetCustomModFieldValue()` return `null` under the following conditions:

* There are no properties or fields in all the mod parts from your mod.
* There is no property or field that goes by the name of the specified name that you plan to execute.
* There is a property or a field, but that item is not static
{% endhint %}

### Listing functions, properties, and fields

You can now list all the available functions, properties, and fields from a specific mod using one of the following functions:

```csharp
public static string[] ListAvailableFunctions(string modName)
public static string[] ListAvailableProperties(string modName)
public static string[] ListAvailableFields(string modName)
```

{% hint style="info" %}
You must specify the main mod name in the above functions, since they use that name to fetch all mod parts and query them for available functions, fields, or properties.

The three functions return an empty array under the following conditions:

* There are no properties or fields in all the mod parts from your mod.
{% endhint %}

### Listing function and set property parameters

You can list all the function and set property parameters in depth by using the following functions:

```csharp
public static ParameterInfo[] GetFunctionParameters(string modName, string functionName)
public static ParameterInfo[] GetSetPropertyParameters(string modName, string propertyName)
```
