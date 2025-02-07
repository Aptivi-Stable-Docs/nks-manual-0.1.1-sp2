---
description: Connecting to your computer remotely...
icon: house-laptop
---

# SSH Connection

<figure><img src="../../../../.gitbook/assets/035-ssh.png" alt=""><figcaption></figcaption></figure>

The SSH protocol is a network protocol that allows you to interact with a remote computer in a secure way, such as remote controlling them, transferring files, and so on. It was engineered by the Internet Engineering Task Force (IETF) to facilitate this task.

Nitrocid KS employs an addon that allows you to initiate SSH connections, including the SFTP client that allows you to securely interactive with the remote computer's files, which has its own documentation here:

{% content-ref url="../more-networking/sftp-client.md" %}
[sftp-client.md](../more-networking/sftp-client.md)
{% endcontent-ref %}

This page talks about how to initiate an SSH connection to the remote computer to open the shell there and to execute the commands remotely.

### Initiating remote shell connection

In order to initiate the remote SSH shell connection, you should use the `sshell` command with the address of the remote computer and the name of the username that you want to log in as. After that, you'll be presented with two options: private key and password.

{% hint style="info" %}
We recommend that you connect to SFTP servers using the private keys, if possible. Also, if you're running an SSH server, such as `dropbear` or `openssh-server`, make sure that you have private key authentication turned on so that you can use the private keys authorization. They are more secure than passwords.
{% endhint %}

Once you provide your key or your password, the SSH addon tries to open a connection to the server and to give you a complete system shell logged in under your username.

### Executing commands remotely

In order to initiate the remote SSH shell connection to execute a single command without having to open the shell, you should use the `sshcmd` command with the address of the remote computer and the name of the username that you want to log in as. After that, you'll be presented with two options: private key and password.

{% hint style="info" %}
We recommend that you connect to SFTP servers using the private keys, if possible. Also, if you're running an SSH server, such as `dropbear` or `openssh-server`, make sure that you have private key authentication turned on so that you can use the private keys authorization. They are more secure than passwords.
{% endhint %}

Once you provide your key or your password, the SSH addon tries to open a connection to the server and to execute a specified command under your username.
