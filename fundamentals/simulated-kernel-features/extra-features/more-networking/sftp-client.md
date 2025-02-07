---
description: Transfer your files securely between your servers and your PC
icon: lock-keyhole
---

# SFTP Client

<figure><img src="../../../../.gitbook/assets/004-sftp.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
As of 0.1.0, this feature has been moved to the kernel addons.
{% endhint %}

This protocol is a modern file transfer protocol that is implemented as part of the Secure SHell (SSH) protocol version 2.0 engineered by the Internet Engineering Task Force (IETF). It allows secure file transfers from the client to the server and from the server to the client.

## How to connect

To connect the client to the SFTP server, you have two ways to initiate a connection to the server, which is connecting directly from the main shell, and connecting inside the SFTP shell.

### Connecting to SFTP from UESH

To connect directly from UESH, please follow the steps:

1. Use the `sftp <server>` command
2. Authenticate using your username and your password or your private key
3. You're connected!

### Connecting to SFTP inside the SFTP shell

To connect to your SFTP server inside the SFTP shell, please follow the steps:

1. Use the `sftp` command
2. Now, execute the `connect <server>` command
3. Authenticate using your username and your password or your private key file
4. You're connected!

## Commands

To see the available commands provided by this shell, consult this page below:

{% content-ref url="../../shells/commands-list.md" %}
[commands-list.md](../../shells/commands-list.md)
{% endcontent-ref %}
