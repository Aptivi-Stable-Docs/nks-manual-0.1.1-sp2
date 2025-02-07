---
description: How to use your FTP client
icon: folder-open
---

# FTP Client

<figure><img src="../../../../.gitbook/assets/003-ftp.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
As of 0.1.0, this feature has been moved to the kernel addons.
{% endhint %}

File Transfer Protocol (FTP) was a standard network protocol used to transfer files from computer to another computer in the network. It used the client-server architecture to manage data and control connections between the source and the target. It normally uses the plain text usernames and passwords to authenticate to the FTP server, but if the server was configured for guest authentication, the client could log in to the server without the username and the password. It first appeared on April 16, 1971.

Various computer applications for both the connection to the FTP server and the hosting service for the protocol have appeared for multiple platforms, like Windows and Linux. The simulated kernel contains the FTP shell which allows you to perform FTP operations on a remote server.

{% hint style="warning" %}
The FTP protocol in general is being replaced by SFTP, although it's kept for historical purposes. This client should be used as a last resort.
{% endhint %}

## How to connect

To connect the client to the FTP server, you have two ways to initiate a connection to the server, which is connecting directly from the main shell, and connecting inside the FTP shell.

### Connecting to FTP from UESH

To connect directly from UESH, please follow the steps:

1. Use the `ftp <server>` command
2. Authenticate using your username and your password
3. Select an FTP profile
4. You're connected!

### Connecting to FTP inside the FTP shell

To connect to your FTP server inside the FTP shell, please follow the steps:

1. Use the `ftp` command
2. Now, execute the `connect <server>` command
3. Authenticate using your username and your password
4. Select an FTP profile
5. You're connected!

## Commands

For the list of available commands, head to the page below:

{% content-ref url="../../shells/commands-list.md" %}
[commands-list.md](../../shells/commands-list.md)
{% endcontent-ref %}
