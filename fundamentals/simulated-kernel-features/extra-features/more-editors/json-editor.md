---
description: Editing your JSON files using just commands
icon: pen-to-square
---

# JSON Editor

<figure><img src="../../../../.gitbook/assets/019-jsoneditorcli.png" alt=""><figcaption></figcaption></figure>

You're looking for an `ed`-like JSON editing experience which allows you to view and edit the JSON file. This is the right place! The `edit` command infers the file type whether it's the text file, the JSON file, or the binary file. It contains many editing tools described in the below section by invoking these commands.

{% hint style="info" %}
Some commands may also be found outside the JSON shell, such as the following:

* `jsonbeautify`: Beautifies the minified JSON file
* `jsonminify`: Minifies the beautified JSON file
* `jsondiff`: Gets a difference between two JSON files
{% endhint %}

## JSON Difference

The JSON difference finding tool can be accessed using the `FindDifferences()` function found in the `JsonTools` class. This allows you to find differences in addition and deletion of any JSON object. The difference tool returns the difference in the following format:

* A JSON object with either a plus (`+`), an asterisk (`*`), or a minus (`-`) sign if the target object is an object.
* A JSON object with either a plus (`+`) or a minus (`-`) sign listing differences in addition and deletion of array elements if the target object is an array.
* A JSON object with a plus (`+`) sign indicating the target object and a minus (`-`) sign indicating the source object.

## Commands

You can consult the below page for the list of JSON editor commands.

{% content-ref url="../../shells/commands-list.md" %}
[commands-list.md](../../shells/commands-list.md)
{% endcontent-ref %}

## Interactive TUI

<figure><img src="../../../../.gitbook/assets/020-jsoneditortui.png" alt=""><figcaption></figcaption></figure>

You can also interactively edit JSON files using the powerful interactive TUI for text editing. You can consult the below page for more information about how to use it:

{% content-ref url="../../editors/text-editor.md" %}
[text-editor.md](../../editors/text-editor.md)
{% endcontent-ref %}
