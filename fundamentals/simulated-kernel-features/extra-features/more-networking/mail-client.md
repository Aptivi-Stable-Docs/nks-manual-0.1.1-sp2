---
description: Manage your messages with this mail client
icon: envelope
---

# Mail Client

<figure><img src="../../../../.gitbook/assets/007-mail.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
As of 0.1.0, this feature has been moved to the kernel addons.
{% endhint %}

E-mails are used to send messages electronically to different contacts using the Internet. The IMAP servers are there to manage the messages and receive them, while the SMTP servers are there to handle sending both unencrypted and encrypted messages to a recipient. These servers are authenticated by the e-mail address and its associated password.

The mail client downloads the messages and manages them locally, but interacts with the two servers to actually manage your messages and check for new e-mail by constantly sending requests for new messages.

## Connecting to your mail address

{% hint style="danger" %}
You can no longer sign in to your Google account as of **May 30, 2022**. We currently don't have plans to fix this.
{% endhint %}

To connect the mail client to your own e-mail address, follow these steps:

1. In the normal UESH shell, execute the `mail <address>` command to provide the credentials needed
2. Wait for a few seconds while your e-mail is being signed in

{% hint style="info" %}
To use the encrypted mail, you need to have both your private key and your recipient's public key. You can manage this by installing [GPG4Win](https://www.gpg4win.org/) or similar software.
{% endhint %}

## Commands

To get a list of available mail commands, consult the page below:

{% content-ref url="../../shells/commands-list.md" %}
[commands-list.md](../../shells/commands-list.md)
{% endcontent-ref %}
