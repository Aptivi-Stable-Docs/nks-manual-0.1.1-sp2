---
description: More networking features
icon: earth-africa
---

# More Networking

In addition to the base networking features that the base Nitrocid kernel features, we have some of the extra features that the networking addons, such as the SFTP shell, ship. Here are the extra features:

{% content-ref url="ftp-client.md" %}
[ftp-client.md](ftp-client.md)
{% endcontent-ref %}

{% content-ref url="sftp-client.md" %}
[sftp-client.md](sftp-client.md)
{% endcontent-ref %}

{% content-ref url="http-client.md" %}
[http-client.md](http-client.md)
{% endcontent-ref %}

{% content-ref url="rss-client.md" %}
[rss-client.md](rss-client.md)
{% endcontent-ref %}

{% content-ref url="mail-client.md" %}
[mail-client.md](mail-client.md)
{% endcontent-ref %}

### Attaching and Detaching

Every single client listed above support attachment to existing connections and detachment from the current connection. When you use one of the clients for the first time, it lets you create a new connection. From there, you have to specify some info about which server you're going to connect, the username, and the password, depending on the client.

When you're connected to any server, you can detach the current connection, which causes you to go back to your shell when you're still connected. In order to attach back to your session, use the same command that you used to create a new connection to connect to the existing server or to create a second new connection.

To detach, simply use the `detach` command to go back to your shell.

#### Speed dial for networked shells

Speed dial allows you to quickly connect to servers with your username, your address, your port, and your custom network configuration. This will save time in having to enter address information manually each time you connect to the same server.

When you invoke a command that launches your networked shell (FTP for example), a selection window shows up with the option to either select an established connection, an option to create a new connection manually, or connect to the saved networks using the speed dial functionality.
