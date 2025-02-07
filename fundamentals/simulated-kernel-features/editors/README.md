---
description: Do you want to edit text?
icon: pencil
---

# Editors

<figure><img src="../../../.gitbook/assets/Beta3-016-Editor.png" alt=""><figcaption></figcaption></figure>

Editors are made to make editing your own files easier. In the modern world, many text editors, like Visual Studio Code, are found to provide nice text editing features, such as the auto-completion which allows you to quickly write any text, the hex editor that allows you to view and edit the contents of a binary file seamlessly, and easier editing using the color coded string tokens to differentiate the string from integers, and much more.

The oldest text editor known to be working with the first Unix system is `ed`, the earliest text editor released on 1973 shipped with the Unix system found on a PDP-7 at AT\&T Bell Labs. It featured commands to manipulate with the text files instead of the visual representation of the file that you can edit easier than before.

The simulated kernel attempts to simulate how `ed` worked, with significant differences in the syntax, allowing you to understand how the first text editor worked faster than before. However, there are three versions of the editor intended to edit different types of files.

If you want to edit the normal text file, choose the below option.

{% content-ref url="text-editor.md" %}
[text-editor.md](text-editor.md)
{% endcontent-ref %}

If you want to edit the binary file using the hex representation, and are careful of what you're doing, choose the below option.

{% content-ref url="hex-editor.md" %}
[hex-editor.md](hex-editor.md)
{% endcontent-ref %}

{% hint style="info" %}
You can also force edit command to launch the editor in a specific type, which can be achieved by appending one of the switches outlined below:

```
edit [-hex|-json|-text|-sql] <file>
```
{% endhint %}
