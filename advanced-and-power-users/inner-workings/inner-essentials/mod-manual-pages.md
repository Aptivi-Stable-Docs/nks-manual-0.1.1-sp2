---
description: Read the manual!
icon: book-open
---

# Mod Manual Pages

Manual pages for your kernel modifications allow you to learn more about your mod as an end user and to document various parts of your mod as a mod developer. This is akin to manual pages on Linux, except that it's more limited in terms of syntaxes, allowing free-form bodies.

## Manual page parsing

At the end of the mod parsing, the manual pages get initialized by the `InitMan()` function, which checks the file extension `.man` and attempts to create a `Manual` class instance.

{% hint style="info" %}
If you want to implement manual pages in your mod, you must have a folder called `<ModName>.dll.manual` in the `KSMods` folder with the `.man` files inside. For example, the tree representation of the directory hierarchy represents a valid way to put your manual files inside for the `MyFirstMod` kernel mod:

```
KSMods:

- MyFirstMod.dll.manual: [/]
  - test.man: 239 B
- MyFirstMod.dll: 14.5 KB
```
{% endhint %}

In turn, this calls the manual checker that parses the entire file. It checks for the following:

* Every manual page starts with the `(*MAN START*)` header
* Every manual page have two fields:
  * `-REVISION:`: Specifies the manual version
  * `-TITLE:`: Specifies the manual page title
* Every manual page must contain one body
  * `-BODY START-` denotes the start of the body
  * `-BODY END-` denotes the end of the body

Optionally, manual pages can contain comments, under the `~~-` prefix. The parser automatically adds comments with the `TODO` constant to the to-do list.

After this is done, the manual page gets added to the available manual pages list, which lets the manual page management functions and the viewer manipulate with the parsed manual instance.

This is an example of a simple manual page file of a mod:

```
(*MAN START*)

-REVISION:1.0
-TITLE:My first mod!

-BODY START-
This is my body!

This is my mod documentation, which hosts the documentation and technical information about what the mod is supposed to do in the kernel.
-BODY END-
```

## Manual page viewer

<figure><img src="../../../.gitbook/assets/148-modmanual.png" alt=""><figcaption></figcaption></figure>

The manual page viewer can be invoked with the `modmanual` command, which takes an argument that should be a valid kernel mod name.

When this command is executed, the manual page viewer TUI starts by listing all the available manual pages that the mod provides. Then, it shows you a brief description in the second pane, which will most likely tell you to press `Shift + I` to get access to more information.

### Controls

The viewer supports these controls:

* `F1`: Info about the selected manual page
* `Shift + I`: Opens the info box containing your mod manual
* `ESC`: Exits the viewer
