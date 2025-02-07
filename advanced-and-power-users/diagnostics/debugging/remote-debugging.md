---
description: Debugging the kernel on LAN
icon: satellite-dish
---

# Remote Debugging

Remote debugging allows you to remotely diagnose the kernel live. It uses TCP networking to listen to the configurable port.

If remote debugging is enabled on your kernel configuration, it starts the remote debugger under the following configuration (for those who use the firewall):

* Socket: TCP
* Port: `3014` (configurable)
* Transfer: Inbound and Outbound

## Connecting

Once the remote debugger starts, you can connect to it using the raw TCP connection to the server. To initiate the connection, select a platform:

### Windows

<figure><img src="../../../.gitbook/assets/095-debug.png" alt=""><figcaption></figcaption></figure>

You can establish a connection to the remote debugger using [PuTTY](https://putty.org/) or an equivalent software. Once you install this, fill the below forms to make a connection:

* Host Name (or IP address): Host of the remote debugger
* Port: `3014` (configurable)
* Type: `Raw`

Click on `Connect` or `Open` and you should be able to see debugging messages from the remote host.

### Linux

You can use the `nc` (netcat) command to connect to a remote debugger using a raw connection. Execute the command in this form: `nc <host> <port>`︎, where:

* `<host>`: Host of the remote debugger
* `<port>`: `3014` (configurable)

You should be able to see debugging messages once the connection to the remote debugger has been established.

## Remote debug chat

<figure><img src="../../../.gitbook/assets/149-rchat.png" alt=""><figcaption></figcaption></figure>

The chatting feature was added to the remote debugger to allow chatting with the other users debugging the same kernel to discuss what is happening in the kernel.

Every device connected to the remote debugger using the provided connection information will have their entries added to the remote debug device configuration file. They'll be told to register their device with a name before they can chat.

The remote debug chat is started along with the remote debugger for nice chatting experience:

* Socket: TCP
* Port: `3015` (configurable)
* Transfer: Inbound and Outbound

Pressing ENTER on the chat connection will post a message to the kernel debugger if chat recording is enabled, causing everyone who connected to the remote chat part of the remote debugger to see the message live.

## Device-only debugging

The debug writer class provides you functions that allow you to write your debugging messages to the devices that are connected to the remote debug session. There are two types of the functions:

### Write to the remote debug

```csharp
public static bool WriteDebugDeviceOnly(DebugLevel Level, string text, bool force, RemoteDebugDevice device, params object[] vars)
```

`WriteDebugDeviceOnly()` writes your debugging message to the devices that are connected to the remote debug facility.

### Write to the remote debug chat

```csharp
public static void WriteDebugChatsOnly(DebugLevel Level, string text, bool force, params object[] vars)
```

`WriteDebugChatsOnly()` writes your debugging message to the devices that are connected to the remote debug chat.
