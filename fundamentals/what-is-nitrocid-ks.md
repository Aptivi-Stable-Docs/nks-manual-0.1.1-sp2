---
description: What is it? How do we simulate the kernel? What aspects are we simulating?
icon: computer
---

# What is Nitrocid KS?

Nitrocid KS attempts to simulate the most basic kernel components and functions, including the hardware parsing, filesystem operations, and drivers. It also demonstrates how the operating system interacts with the kernel.

Operating systems usually interact with the kernel to send and receive I/O operations from hardware. The simulator attempts to simulate that.

For example, one of the kernel components is the booting up stage, where the kernel entry point was invoked by GRUB after loading the entire kernel to the system memory. GRILO and Nitrocid KS interact with each other to simulate the booting process of the kernel in a way that it works just like how a computer would boot, though the simulator can also run independently. A real kernel can't run by itself; it needs a bootloader.

The name of the kernel simulated by the simulator is <mark style="color:orange;">Nitrocid Kernel (A portmanteau of Nitric Acid - HNO₃), codename Project Decompose</mark>.

## What features do we simulate?

To see the kernel features that will blow your mind, go to the page linked below:

{% content-ref url="simulated-kernel-features/" %}
[simulated-kernel-features](simulated-kernel-features/)
{% endcontent-ref %}
