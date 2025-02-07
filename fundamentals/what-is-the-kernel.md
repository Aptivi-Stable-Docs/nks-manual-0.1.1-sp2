---
description: In general...
icon: corn
---

# What is the Kernel?

The Kernel is the core of the operating system that powers complete control over the entire system and is memory-resident. It allows interactions between hardware and software. It's one of the first programs that get loaded after the bootloader stage.

The kernel controls hardware and their resources, such as the input and output operations, using the device drivers. It also handles the rest of the startup, including memory, peripherals, and I/O requests from software and translates them into CPU instructions.

It also performs its own tasks, such as handling hardware interrupts, managing hardware, and running processes, in the kernel space that is protected from access by external applications.

Kernel Simulator attempts to simulate how the kernel works and how the operating system interacts with the kernel. To deeply explain how, see the next page.

{% content-ref url="what-is-nitrocid-ks.md" %}
[what-is-nitrocid-ks.md](what-is-nitrocid-ks.md)
{% endcontent-ref %}
