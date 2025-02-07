---
description: Changing how the encryption works
icon: plug-circle-bolt
---

# Encryption Drivers

<figure><img src="../../../../.gitbook/assets/122-inner.png" alt=""><figcaption></figcaption></figure>

The encryption driver is one of the supported driver types on Nitrocid KS. These drivers allow you to change how the encryption works, thus earning dynamic encryption improvements.

The console drivers have the following characteristics:

* Interface: `IEncryptionDriver`
* Base class: `BaseEncryptionDriver`

The encryption drivers have the following functions that you can optionally override below:

{% code title="IEncryptionDriver.cs" lineNumbers="true" %}
```csharp
abstract string EmptyHash { get; }
abstract int HashLength { get; }
abstract Regex HashRegex { get; }
abstract string GetEncryptedString(string str);
abstract string GetEncryptedFile(Stream stream);
abstract string GetEncryptedFile(string Path);
abstract bool VerifyHashFromHash(string FileName, string ExpectedHash, string ActualHash);
abstract bool VerifyHashFromHashesFile(string FileName, string HashesFile, string ActualHash);
abstract bool VerifyUncalculatedHashFromHash(string FileName, string ExpectedHash);
abstract bool VerifyUncalculatedHashFromHashesFile(string FileName, string HashesFile);
```
{% endcode %}

The `EncryptionDriverTools` class contains tools to get all the encryption drivers and their names and set a encryption driver as a default. The driver management tools also allow you to do the same thing, though you'll have to specify the driver type.

{% hint style="warning" %}
Nitrocid supplies the extra encryption drivers using the kernel addons, but some of them require your system to be running below:

* `SHA256Enhanced` (Windows 11 24H2 or later, Linux with OpenSSH 1.1.1 or later, macOS not supported)
* `SHA384Enhanced` (Windows 11 24H2 or later, Linux with OpenSSH 1.1.1 or later, macOS not supported)
* `SHA512Enhanced` (Windows 11 24H2 or later, Linux with OpenSSH 1.1.1 or later, macOS not supported)
{% endhint %}
