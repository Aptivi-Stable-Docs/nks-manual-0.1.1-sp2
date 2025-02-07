---
description: Making your own language...
icon: earth-americas
---

# Language Studio

<figure><img src="../../../.gitbook/assets/043-langstudio.png" alt=""><figcaption></figcaption></figure>

You can make your own language using the language studio, which allows you to make your own kernel translation strings file for your own custom language. You can usually use this program to translate the kernel to your own native language. However, translation takes a lot of time, so you'll need to have some patience and focus.

Once you're done using the language studio, it provides you with an option to save your translation work. Afterwards, you can use the language manager, `langman`, to load your custom language using `langman <load> <customlanguagename>`. Once your language is loaded, you can set the language to your custom one using the `settings` command.

## How to use

In order to be able to use the language studio, you'll need a folder with a text file containing English strings, called `eng.txt`, at least one or more text files with the three-lettered language names as their file names, such as `spa.txt`, and a language metadata file containing information about a language, called `Metadata.json`. For example, `AddonTranslations` contains the following files as of 0.1.0:

```
-a----         2/29/2024  11:39 AM         232234 arb-T.txt
-a----         2/29/2024  11:39 AM         184438 arb.txt
-a----         2/29/2024  11:39 AM         133504 chi-T.txt
-a----         2/29/2024  11:39 AM         198677 chi.txt
-a----         2/29/2024  11:39 AM         134067 cnt-T.txt
-a----         2/29/2024  11:39 AM         198850 cnt.txt
-a----         2/29/2024  11:39 AM         171209 dtc.txt
-a----         2/29/2024  11:39 AM         149696 eng.txt
-a----         2/29/2024  11:39 AM         150170 enk.txt
-a----         2/29/2024  11:39 AM         192963 fre.txt
-a----         2/29/2024  11:39 AM         255617 frs.txt
-a----         2/29/2024  11:39 AM         186980 ger.txt
-a----         2/29/2024  11:39 AM         336842 grk.txt
-a----         2/29/2024  11:39 AM         404558 ind-T.txt
-a----         2/29/2024  11:39 AM         176800 ind.txt
-a----         2/29/2024  11:39 AM         179106 ita.txt
-a----         2/29/2024  11:39 AM         222422 jpn.txt
-a----         2/29/2024  11:39 AM         189789 kor-T.txt
-a----         2/29/2024  11:39 AM         184358 kor.txt
-a----         2/29/2024  11:39 AM         145953 ltn.txt
-a----         2/29/2024  11:39 AM           3329 Metadata.json
-a----         2/29/2024  11:39 AM         174317 ptg.txt
-a----         2/29/2024  11:39 AM         181449 rmn.txt
-a----         2/29/2024  11:39 AM         295653 rus-T.txt
-a----         2/29/2024  11:39 AM         174255 rus.txt
-a----         2/29/2024  11:39 AM         181854 spa.txt
```

You can learn more about how to make this folder with this structure by going to the translation internals here:

{% content-ref url="../../../advanced-and-power-users/inner-workings/multilingual-kernel/custom-languages.md" %}
[custom-languages.md](../../../advanced-and-power-users/inner-workings/multilingual-kernel/custom-languages.md)
{% endcontent-ref %}

Once you point `mklang` to a directory containing these files, you'll be taken to a screen that allows you to select an English string to be translated. You can select one of them by moving up or down and pressing `ENTER`. Then, you'll be provided with a list of languages and their translations like below:

<figure><img src="../../../.gitbook/assets/164-langstudio.png" alt=""><figcaption></figcaption></figure>

Select any of them, and you'll be presented with an input text telling you to enter a translation of the selected string in the selected language. Once done, press `ENTER`, and your translation will be placed.

If you're done translating the same string in all the languages, you can go back by highlighting `Go Back`. Repeat this process for every single string that you want to translate.

* You can also add a new string by highlighting the `New string` option. You'll be prompted to write the string that you want translated. You can then translate the same string for all the languages.
* If you want to remove a string, you'll have to choose the `Remove string` option. You'll be prompted to select a string that you want removed.

If you want to save the translations and make them usable as a language, you can choose `Save translations`, and your translation files will be placed under the `Output` folder.

{% hint style="info" %}
Alternatively, you can launch the language studio TUI by passing the `-tui` switch. Consult the keystrokes list by pressing the `K` key inside the TUI for help.
{% endhint %}
