---
description: Do you want an app to access your files?
icon: user-lock
---

# Privacy Consents

For controlled access to parts of your system, like your files, we've introduced privacy consents feature which allows you to choose whether to allow a certain operation to be done to your files, data, etc. This increases privacy, especially when apps try to abuse privacy-sensitive features to do something malignant.

Nitrocid defines these permissions to require your consent before continuing the operation:

* `FilesystemRead`
  * An application or mod tries to read from a file
* `FilesystemWrite`
  * An application or mod tries to write to a file

{% hint style="info" %}
Please note that your consent is required only for above accesses that are done through the Nitrocid API for each user.

If you're a mod developer and care about privacy of your users, use the Nitrocid API where checks are done to ensure that users have consented to the above operations.

If the user declines the consent, an exception will be thrown to the caller, depending on how a particular Nitrocid API function is coded.
{% endhint %}

In your mod code, you can ask for consent using the below function:

```csharp
public static bool ConsentPermission(ConsentedPermissionType consentType)
```

This is useful if you want your mod to not ask you for your consent in the middle of its operation, but rather when it starts. Additionally, you can ask for consent in the mod's introductory screen, just like apps built for Android Marshmallow or higher.
