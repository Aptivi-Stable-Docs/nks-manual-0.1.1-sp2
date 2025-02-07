---
description: Handles all your RSS feeds
icon: square-rss
---

# RSS Client

<figure><img src="../../../../.gitbook/assets/005-rss.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
As of 0.1.0, this feature has been moved to the kernel addons.
{% endhint %}

RDF Site Summary or Really Simple Syndication (RSS) is a web feed that allows users and applications to get updates from the web sites that support RSS feeds. It consists of elements that contain information about the latest feeds and their summary, date published, title, and so on. It was hugely popular between 2005 and 2006.

The simulated kernel contains the RSS client which gets updates from any RSS feed, as well as list and read the feeds. It automatically synchronizes the feed every set interval. The default interval for the refresh is 1 minute.

## Connecting to a feed

To connect to any RSS feed, get any RSS feed URL and execute the `rss` command with the URL. The usage of this command is `rss [-tui] <feed>`.

If you passed the `-tui` switch, it'll open the interactive TUI to your favorite RSS feed URL listing all your articles. Here are the controls:

* `F1`: Shows you more information about the article that you're reading
* `F2`: Opens your web browser to the article
* `F3`: Refreshes the list of articles for new updates

## Commands

These below commands can be found by consulting the below page:

{% content-ref url="../../shells/commands-list.md" %}
[commands-list.md](../../shells/commands-list.md)
{% endcontent-ref %}
