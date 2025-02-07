---
description: Files and folders are essential for your daily computing usage
icon: folder-open
---

# Files and Folders

<figure><img src="../../../.gitbook/assets/155-listtree.png" alt=""><figcaption></figcaption></figure>

Files and folders are created to organize your documents, photos, music, videos, and other types of files. They are useful for many purposes, like organizing your archives, your albums, your backups, your personal files, and other types.

Nitrocid KS simulates this component with the help of kernel drivers using your host computer's filesystem to perform common file operations, such as copying, moving, deleting, editing, and many others. In addition, it also supports advanced features, like file content type detection and file lock check.

To see how it works, consult the below page to take you to the inner workings of the Nitrocid kernel filesystem.

{% content-ref url="../../../advanced-and-power-users/inner-workings/inner-essentials/nitrocid-filesystem.md" %}
[nitrocid-filesystem.md](../../../advanced-and-power-users/inner-workings/inner-essentials/nitrocid-filesystem.md)
{% endcontent-ref %}

Meanwhile, you can check out the list of available commands here:

{% content-ref url="../shells/commands-list.md" %}
[commands-list.md](../shells/commands-list.md)
{% endcontent-ref %}

## Interactive file manager

<figure><img src="../../../.gitbook/assets/063-ifm.png" alt=""><figcaption></figcaption></figure>

This interactive TUI is a powerful file manager that allows you to view what's inside the folders, as well as performing operations, like copying, moving, or deleting, on files and folders, just like [Total Commander](https://www.ghisler.com/index.htm) or [Midnight Commander](https://midnight-commander.org/).

The file management TUI can be accessed using the `ifm` command. You can use the following keys to manipulate with the files here:

* `Enter`: Go to a folder or open a file
* `F1`: Copy a folder or file to the other pane's current working directory
* `F2`: Move a folder or file to the other pane's current working directory
* `F3`: Deletes a file or folder
* `F4`: Goes one directory up
* `F5`: Shows an information box containing file or directory information
* `F6`: Allows you to select a local folder in the current pane
* `SHIFT + F1`: Allows you to enter a directory to copy the selected file to
* `SHIFT + F2`: Allows you to enter a directory to move the selected file to
* `F9`: Allows you to rename a selected file or folder to another name
* `F10`: Allows you to make a new folder
* `F11`: Gets the file checksum
* `F12`: Verifies the selected file
* `P`: Previews a file in a non-modal informational box
* `Tab`: Switches panes
* `Esc`: Exits the application

{% hint style="info" %}
You can turn on/off the file size display on the status in the kernel settings, but this will affect all other applications that use this settings entry.
{% endhint %}

### Copying or moving files

To copy or to move files from the current pane to the current working directory in the another pane, press `F1` or `F2` to perform this operation. You may need to direct the other pane to the target directory that you want to move your files or folders to before performing this operation.

You can also copy or move files to any other directory by pressing the `SHIFT + F1` or the `SHIFT + F2` key to open an informational box that allows you to write a path to a folder that you want to copy or move the selected file or folder to.

### Removing files

To remove files or folders from the directory in the current pane, you can press `F3`. Please note that you can not reverse this operation, as it actually deletes a file from your storage device; it doesn't move the deleted file to your system's recycle bin.

### Navigating directories

In the interactive file manager, you can navigate the whole storage device by going to desired folders, such as your source code directory located in a secondary hard drive. If you press `ENTER` on a directory, you'll open it, allowing you to see its contents. Pressing `F4` causes you to go back one level up - that is, going back to the previous directory.

<figure><img src="../../../.gitbook/assets/151-ifm.png" alt=""><figcaption></figcaption></figure>

However, if you want to select a directory to navigate to by selecting a directory, you can press `F6` to open up another interactive TUI that allows you to navigate to folders and select the desired folder. Pressing `Spacebar` on a folder causes you to select a folder that you want to open. If you want to open a folder using just a path or if you want to go to another drive, you can use `F6` to open an interactive input infobox that allows you to write a full path to a desired directory.

If you selected a folder and pressed `ESC` to exit the selector, the interactive file manager will open a folder of your choice.

### File or folder information

<figure><img src="../../../.gitbook/assets/152-ifm.png" alt=""><figcaption></figcaption></figure>

You can get information about either a file or a folder that you have selected in the current pane by pressing the `F5` key. For directories, you'll be able to get the following information:

* `Name`
* `Full name`
* `Size`
* `Creation time`
* `Last access time`
* `Last write time`
* `Attributes`
* `Parent directory`

<figure><img src="../../../.gitbook/assets/153-ifm.png" alt=""><figcaption></figcaption></figure>

However, if you press `F5` on a selected file, you'll be able to get the following information:

* `Name`
* `Full name`
* `File size`
* `Creation time`
* `Last access time`
* `Last write time`
* `Attributes`
* `Where to find`
* `Binary file`
* `MIME metadata`
* `.NET assembly info`
* `Extra information`

{% hint style="info" %}
The file info supports extra information showcase that you can implement by making a format handler with appropriate parameters.
{% endhint %}

### Renaming files

The interactive file manager also allows you to rename a file or a folder that is selected. Press `F9` to write a new file name.

{% hint style="info" %}
The MIME metadata changes after you rename a file due to how Nitrocid parses this information. More accurate description of a file will come soon under a new library.
{% endhint %}

### Making a new folder

You can also easily make a new folder by pressing `F10` to allow you to write a name of the new folder. It lets you make a new folder in the current directory with a desired name.

### Hash information

<figure><img src="../../../.gitbook/assets/160-ifmhash.png" alt=""><figcaption></figcaption></figure>

By pressing `F11`, you can get a hash representation of a selected file under an algorithm that you select, assuming that this algorithm exists in the encryption driver manager that you can learn more about here:

{% content-ref url="../../../advanced-and-power-users/inner-workings/inner-essentials/kernel-drivers/encryption-drivers.md" %}
[encryption-drivers.md](../../../advanced-and-power-users/inner-workings/inner-essentials/kernel-drivers/encryption-drivers.md)
{% endcontent-ref %}

By pressing `F12`, you can verify the hash of a file by pasting in the expected hash. Note that the expected hash must be in the same algorithm as the driver that you've selected.

{% hint style="info" %}
Note that `F11` may be used in the Windows command prompt. In the Windows Terminal, disable the `F11` keybinding by going to `Settings` > `Actions`.
{% endhint %}

### File preview

<figure><img src="../../../.gitbook/assets/154-ifm.png" alt=""><figcaption></figcaption></figure>

By pressing `P`, you'll be able to preview either a binary or a text file inside an informational box. You can use `UP` and `DOWN` arrows to navigate the file by one line, `Page Up` and `Page Down` to navigate by one page, or `Q` to exit the informational box.
