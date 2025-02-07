---
description: How do the permissions work?
icon: key
---

# The Permissions

Permissions are the authorities that are permitted to the user. It allows the kernel user to gain slightly more power than the absolute normal user.

When the kernel starts up, it reads the `permissions` array in the configuration for each user. If it finds a permission in the array, it calls the `PermissionsTools.GrantPermission()` function under the `Nitrocid.Security.Permissions` namespace.

{% hint style="info" %}
The call to the above function requires the `ManagePermissions` permission to be granted in the current user.
{% endhint %}

This function gets all the permissions that may have been fused together by the call to this function (adding multiple permissions at once) and checks them one by one to see if it's granted. If not yet granted, it adds the permission to the granted permissions list. It then saves the changes to the configuration file.

In the case of a user being part of a group that is granted several permissions, a user is automatically granted permissions from that group. This is called inheritance, since it's basically an inheritance of permissions from a group to all the users that are members of that group.

The function that does the reverse operation of granting permissions (revoking the permission) is `RevokePermission()`.

{% hint style="info" %}
If you want your mod to request additional permissions, we prefer to use the `Demand()` function which handles multiple permissions at once.
{% endhint %}

You can also manually demand one permission type by issuing the `IsPermissionGranted()` function like this:

```csharp
if (!PermissionsTools.IsPermissionGranted(PermissionTypes.type))
    throw new KernelException(KernelExceptionType.PermissionDenied);
```

...where the `type` is one of the following types:

* `ManagePower`: Allows the user to manage power
  * Flag value is 1
* `ManagePermissions`: Allows the user to manage permissions
  * Flag value is 2
* `RunStrictCommands`: Allows the user to run strict commands
  * Flag value is 4
* `ManageFilesystem`: Allows the user to perform the filesystem operations
  * Flag value is 8
* `ManipulateSettings`: Allows the user to manipulate with the kernel settings
  * Flag value is 16
* `ExecuteScripts`: Allows the user to execute UESH scripts
  * Flag value is 32
* `ExecuteProcesses`: Allows the user to execute processes
  * Flag value is 64
* `ManageUsers`: Allows the user to manage the users
  * Flag value is 128
* `ManageMods`: Allows the user to manage the kernel mods
  * Flag value is 256
* `ManageGroups`: Allows the user to manage the user groups
  * Flag value is 512
* `IntermodCommunication`: Allows the user to run mod commands that depend on inter-mod communication
  * Flag value is 1024
* `OpenAdminShell`: Allows the user to open an administrative shell
  * Flag value is 2048
* `OpenDebugShell`: Allows the user to open a debug shell
  * Flag value is 4096
* `InteraddonCommunication`: Allows the user to run commands that depend on inter-addon communication
  * Flag value is 8192
* `UseSudo`: Allows the user to use the sudo command
  * Flag value is 16384
