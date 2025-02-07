---
description: You're connected to the network!
icon: earth-europe
---

# Network Tools

The network tools allow you to perform basic network operations that Nitrocid KS supports. These tools are used by the basic network features, which you can basically consult its documentation page at:

{% content-ref url="../../../fundamentals/simulated-kernel-features/networking.md" %}
[networking.md](../../../fundamentals/simulated-kernel-features/networking.md)
{% endcontent-ref %}

The networking tools provide you various properties and functions that can be useful for your kernel mods.

## Properties

These properties can be used by your kernel mods:

* `HostName`: The kernel host name
* `DownloadRetries`: The number of retries for a download operation
* `UploadRetries`: The number of retries for an upload operation
* `PingTimeout`: The ping timeout specified in milliseconds
* `NetworkAvailable`: Specifies whether the network is available or not
* `InternetAvailable`: Specifies whether the Internet connection is available or not
* `ShowProgress`: Shows the progress bar while downloading using the normal network transfer functions

## Functions

There are several base networking functions that are useful for your mods, such as pinging a server from either an IP address or an associated hostname.

### Pinging a Server

<figure><img src="../../../.gitbook/assets/142-inner.png" alt=""><figcaption></figcaption></figure>

You can ping a server using the `PingAddress()` function, which contains several versions of the function that provide you different options:

{% code title="NetworkTools.cs" lineNumbers="true" %}
```csharp
public static PingReply PingAddress(string Address, PingOptions options = null)
public static PingReply PingAddress(string Address, int Timeout, PingOptions options = null)
public static PingReply PingAddress(string Address, int Timeout, string text, PingOptions options = null)
public static PingReply PingAddress(string Address, int Timeout, byte[] Buffer, PingOptions options = null)
```
{% endcode %}

These functions provide you with different options:

* The first version specifies a target address with ping options
* The second version has the same features as the first version, with an ability to specify a timeout
* The third version has the same features as the second version, with an ability to specify a custom text
* The fourth version has the same features as the second version, with an ability to specify a byte buffer representing data

### Changing the kernel hostname

You can change the kernel hostname using the `ChangeHostname()` function, but that won't change your computer's hostname; it only affects the simulated kernel.

{% code title="NetworkTools.cs" lineNumbers="true" %}
```csharp
public static void ChangeHostname(string NewHost)
```
{% endcode %}

### Getting a file name from the URL

The base networking class contains a function that allows you to get a file name right from the URL. This makes getting a file name from the URL easier as it strips the URL to the last element that usually represents the file name, such as `index.html`, and also strips the query strings (those beginning with `?` that represent arguments delimited with `&`).

As a result, this function returns a file name taken from the URL. You can access this function using `GetFilenameFromUrl()`.

{% code title="NetworkTools.cs" lineNumbers="true" %}
```csharp
public static string GetFilenameFromUrl(string Url)
```
{% endcode %}

### Getting online devices

For online devices, you can scan your whole network by pinging each individual machine with fast network pinging. There is a function dedicated to getting online devices, called `GetOnlineDevicesInNetwork()`.

{% code title="NetworkTools.cs" lineNumbers="true" %}
```csharp
public static IPAddress[] GetOnlineDevicesInNetwork()
```
{% endcode %}
