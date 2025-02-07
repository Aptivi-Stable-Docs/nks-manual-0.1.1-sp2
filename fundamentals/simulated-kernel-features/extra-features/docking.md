---
description: You can now dock your screen!
icon: image
---

# Docking

<figure><img src="../../../.gitbook/assets/042-dock.png" alt=""><figcaption></figcaption></figure>

When it comes to docking your laptop, your desktop, or your tablet, it becomes like an information center that shows you latest information about various things that happens in the whole world, such as the current time and date, the world clock, the latest match information about your favorite sports team, and so on.

Information centers tend to require no input from the user, because they generally update themselves to get the latest information in real time, usually from the Internet. Sometimes, they allow you to touch its screen to make changes to the type of information you want to show.

Nitrocid KS attempts to simulate this concept by implementing the non-touch version of the information center that only gives you latest information about various things, such as the time and the date. This is accompanied by the usage of the brand-new screen feature to gain flexibility in regards to screen resizes. Its documentation can be found here to see the magic behind this feature:

{% content-ref url="https://app.gitbook.com/s/G0KrE9Uk2AiblqjWtpAo/usage/console-tools/console-screen" %}
[Console Screen](https://app.gitbook.com/s/G0KrE9Uk2AiblqjWtpAo/usage/console-tools/console-screen)
{% endcontent-ref %}

### How do I dock my screen?

You can dock your screen to turn it to an information center using the `dock` command, giving it an ID of the dock. The following docks can be used:

<table><thead><tr><th width="161">Name</th><th width="163">ID</th><th>Description</th></tr></thead><tbody><tr><td>Digital Clock</td><td><code>DigitalClock</code></td><td>Shows you a digital clock that updates itself</td></tr><tr><td>Analog Clock</td><td><code>AnalogClock</code></td><td>Shows you an analog clock that updates itself</td></tr><tr><td>Emoji</td><td><code>Emoji</code></td><td>Cycles between emojis</td></tr></tbody></table>

Once you dock your screen, depending on the dock used, you can follow the instructions usually given by the dock mode to exit it. The screensaver won't be launched during the docking period. Docks use the widget system to render, so you can consult the page below for more information.

{% content-ref url="../lockscreen-widgets.md" %}
[lockscreen-widgets.md](../lockscreen-widgets.md)
{% endcontent-ref %}
