---
description: We need to verify your files.
icon: key-skeleton
---

# Hashing and Encryption

<figure><img src="../../../.gitbook/assets/157-sum.png" alt=""><figcaption></figcaption></figure>

When it comes to verifying the file integrity, the file hashing feature of Nitrocid KS's simulated filesystem comes into play. It features built-in file hashing techniques to allow you to check the sanity of your files. The kernel file encryption features are handled by your selected encryption kernel driver, which can be found here:

{% content-ref url="../../../advanced-and-power-users/inner-workings/inner-essentials/kernel-drivers/encryption-drivers.md" %}
[encryption-drivers.md](../../../advanced-and-power-users/inner-workings/inner-essentials/kernel-drivers/encryption-drivers.md)
{% endcontent-ref %}

## Hashing

This part of the entire encryption feature for files is a crucial part, because it involves using your encryption driver to let you get a hash sum for a file or a text. This is also used to calculate the hash sum for a file prior to verification to check to see if your file is tampered with or not.

The following commands are available for you to try out:

* `sumfile`: Allows you to get a hash sum for a single file
* `sumfiles`: Allows you to get a hash sum for a single directory
* `sumtext`: Allows you to get a hash sum for a text

In the `Encryption` class found in the `Nitrocid.Drivers.Encryption` namespace, you can access the hashing tools to give your mods ability to use the current encryption driver to encrypt your files and your texts.

The following functions are available:

* `GetEncryptedString()`
* `GetEncryptedFile()`

## Verification

<figure><img src="../../../.gitbook/assets/158-sum.png" alt=""><figcaption></figcaption></figure>

You can also use the verification feature in both the main command-line shell and in your mod's code to allow you to check a file to see if the two hashes (calculated or not) match or not. This works under the following two conditions:

* If the verification part concludes that the two hashes match, your file is fine.
* If the verification part concludes that the two hashes don't match, your file is tampered with.

The following commands are available for you to try out:

* `verify`: Allows you to verify your file or your hash

In the `HashVerifier` class found in the `Nitrocid.Drivers.Encryption` namespace, you can access the verifier tools to give your mods ability to use the current encryption driver to verify the hash sum of your files and your texts.

The following functions are available:

* `VerifyHashFromHashesFile()`
* `VerifyHashFromHash()`
* `VerifyUncalculatedHashFromHashesFile()`
* `VerifyUncalculatedHashFromHash()`
* `VerifyHashPlain()`

## Miscellaneous

<figure><img src="../../../.gitbook/assets/159-sum.png" alt=""><figcaption></figcaption></figure>

In both the above classes, you can also access the miscellaneous hash and verification tools. The following tools can be used in your mods:

### Hash verifier

* `GetExpectedHashLength()`: Gets the expected hash length according to the algorithm.

### Hashing

* `GetArrayEnc()`: Gets a string representation of a byte array.
* `GetEmptyHash()`: Gets a hash sum for an empty string.
