---
description: Choose where to go.
icon: hand-back-point-up
---

# Boot Choices

Nitrocid's internal bootloader simulates the boot choices as in GRUB and LILO. These allow you to select any operating system to boot from in the real-world. The simulated bootloader lets you select any kernel environment to run the main entry point.

The bootloader has several controls, including the custom ones that your boot style can make use of.

## Controls

The general controls that this bootloader makes use of (and are non-overridable) are listed below:

* `UP ARROW`: Chooses the boot entry above the current selection. If the current selection is the first selection, it'll go to the last entry available.
* `DOWN ARROW`: Chooses the boot entry below the current selection. If the current selection is the last selection, it'll go to the first entry available.
* `HOME`: Goes to the first entry
* `END`: Goes to the last entry
* `SHIFT + H`: Shows the help information (lists available controls for both built-in and boot style-defined keybindings)
* `ENTER`: Boots to the selected kernel environment.
