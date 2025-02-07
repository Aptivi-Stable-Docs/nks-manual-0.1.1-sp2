---
description: Edit your hex files reliably and in bytes
icon: binary
---

# Hex Editor

<figure><img src="../../../.gitbook/assets/Beta3-018-HexEditor.png" alt=""><figcaption></figcaption></figure>

You're looking for an `ed`-like hex editing experience which allows you to view and edit the binary file. This is the right place! The `edit` command infers the file type whether it's the text file, the JSON file, or the binary file. It contains many editing tools described in the below section by invoking these commands.

{% hint style="danger" %}
Unless you know what you're doing with the binary file, editing such file in this way will lead to data corruption or data loss in the targeted file.
{% endhint %}

## Commands

You can consult the below page for the list of hex editor commands.

{% content-ref url="../shells/commands-list.md" %}
[commands-list.md](../shells/commands-list.md)
{% endcontent-ref %}

## Interactive TUI

<figure><img src="../../../.gitbook/assets/Beta3-019-HexEditor.png" alt=""><figcaption></figcaption></figure>

In addition to the hex editor being a shell, you can switch to an alternative mode when you're in the shell by executing the `tui` command. This way, you'll get access to the fully interactive hex editor.

The interactive hex editor works like any other interactive hex editor. However, it's just that it's different in terms of how it operates.

* To exit, press `ESC`.
* To get a list of available keybindings, press `K`.

The currently selected byte is highlighted with the green color, depending on your kernel theme. Here are some of the common controls:

| Key           | Action                                                                        |
| ------------- | ----------------------------------------------------------------------------- |
| `F1`          | Inserts a new byte to the selected byte number.                               |
| `F2`          | Removes a selected byte number                                                |
| `F3`          | Replaces the currently selected byte with another byte value                  |
| `Shift + F3`  | Replaces all instances of the currently selected byte with another byte value |
| `F4`          | Shows you a box about the currently selected byte number information          |
| `Left Arrow`  | Advances one byte backwards                                                   |
| `Right Arrow` | Advances one byte forwards                                                    |
| `Up Arrow`    | Advances 16 bytes backwards                                                   |
| `Down Arrow`  | Advances 16 bytes forwards                                                    |
| `Page Up`     | Goes to the previous page                                                     |
| `Page Down`   | Goes to the next page                                                         |
| `Home`        | Goes to the beginning of the binary file                                      |
| `End`         | Goes to the end of the binary file                                            |
