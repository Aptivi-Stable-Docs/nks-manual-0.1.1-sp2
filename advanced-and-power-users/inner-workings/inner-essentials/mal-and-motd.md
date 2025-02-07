---
description: Message of the Day before and after login
icon: message
---

# MAL and MOTD

Messages of the day were used before and after logging in to your user account to give you a warm greeting, a funny quote, or a text of your choice. This gives you a subtle information about your message of the day.

Nitrocid initializes your MAL and your MOTD file when the kernel boots for the first time by saving them with the defaults. The next time the kernel finds your MAL and your MOTD files, the kernel uses them to read these messages and display them before and after logging in to your user account.

## Basics

This section covers the basic MOTD and MAL messages.

### Initialization

```csharp
public static void InitMal()
public static void InitMotd()
```

For initialization, the `ReadMal()` and the `ReadMotd()` functions call these functions, `InitMal()` and `InitMotd()` respectively, when their associated files aren't found. The two functions check to see if their respective files are found in the Nitrocid KS's configuration directory and makes them if they're not found.

When automatically creating them, they'll get created with the following contents:

* MOTD: `Welcome to Nitrocid Kernel!`
* MAL: `Enjoy your day, <user>!`

### Setting

```csharp
public static void SetMal(string Message)
public static void SetMotd(string Message)
```

As for saving the current messages, you can use the `SetMal()` and the `SetMotd()` functions, providing it the message that you want to save. Once done, the kernel will read your updated MOTD and MAL messages.

{% hint style="info" %}
Your message can contain placeholders, such as \<user>. To learn more about the text placeholders, you can consult the below page:

[kernel-placeholders.md](kernel-placeholders.md "mention")
{% endhint %}

### Reading

```csharp
public static void ReadMal()
public static void ReadMotd()
```

When it comes to reading, the `ReadMal()` and `ReadMotd()` functions do everything to try to read the MAL and the MOTD messages and set them to the parsed MOTD and MAL message properties. The next time the current message is queried through the MotdMessage and the MalMessage properties, that message will be returned.

{% hint style="info" %}
If the messages haven't been read, they'll return a fallback value.
{% endhint %}

## Dynamic MOTD and MAL

The dynamic MOTD and MAL is more flexible than the basic MOTD and MAL messages grabbed from your message files. The only way to define them is to register them to the list of dynamic MOTD and MAL message list.

You can register such messages using this handy function:

```csharp
// For MOTD
public static void RegisterDynamicMotd(Func<string> dynamicMotd)

// For MAL
public static void RegisterDynamicMal(Func<string> dynamicMal)
```

Similarly, you can remove your dynamic message, provided that you've kept a copy of the function variable, using the following functions:

```csharp
// For MOTD
public static void UnregisterDynamicMotd(Func<string> dynamicMotd)

// For MAL
public static void UnregisterDynamicMal(Func<string> dynamicMal)
```

{% hint style="info" %}
You'll need to register them when your kernel mod starts up so that the kernel recognizes your custom MOTD and MAL dynamic messages.

Support for the dynamic MOTD is up to the login handler.
{% endhint %}
